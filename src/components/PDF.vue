<template>
  <div class="pdf">
    <button class="btn btn-secondary" @click="generate()">Скачать PDF</button>
  </div>
</template>

<script>
import pdfMake from "pdfmake/build/pdfmake";
import pdfFonts from "pdfmake/build/vfs_fonts";
pdfMake.vfs = pdfFonts.pdfMake.vfs;
import { bus } from "../main";

export default {
  props: {
    contractNumber: {
      type: String,
      default: "",
    },
    numberScore: {
      type: String,
      default: "",
    },
    dateToday: {
      type: String,
      default: "",
    },
    items: {
      type: Array,
      default: () => [],
    },
  },

  data() {
    return {
      organization: {},
      counterparty: {},
    };
  },
  methods: {
    generate() {
      
      var docDefinition = {
        content: [
          {
            style: "tableExample",
            table: {
              widths: [300, 30, 150],
              heights: [12, 12, 12],
              body: [
                [
                  {
                    rowSpan: 2,
                    text:
                      this.organization.fileds.nameBank + "\n" +
                      "\n" +
                      "Банк получателя",
                  },
                  "БИК",
                  {
                    rowSpan: 2,
                    text:
                      this.organization.fileds.bik +
                      "\n" +
                      "\n" +
                      this.organization.fileds.korChet,
                  },
                ],
                ["", "Сч.№"],
                [
                  "ИНН " +
                    this.organization.fileds.inn +
                    "                | КПП " +
                    this.organization.fileds.kpp,
                  { rowSpan: 2, text: "Сч.№" },
                  { rowSpan: 2, text: this.organization.fileds.rasChet },
                ],
                [this.organization.fileds.name + "\n" + "\n" + "Получатель ", "", ""],
              ],
            },
          },
          {
            text: "---------------------------------------------------------------------------------------------------------------------------------------------------------",
          },
          {
            text:
              "Счет на оплату № " + this.numberScore + " от " + this.dateToday,
            style: "header",
          },
          {
            text: [
              "Поставщик: ",
              {
                text:
                  this.organization.fileds.name +
                  " ИНН " +
                  this.organization.fileds.inn +
                  " " +
                  this.organization.fileds.adress +
                  " " +
                  this.organization.fileds.telefon,
                style: "tableHeader",
              },
            ],
          },
          { text: " " },
          {
            text: [
              "Получатель: ",
              {
                text:
                  this.counterparty.fileds.name +
                  " ИНН " +
                  this.counterparty.fileds.inn +
                  " " +
                  this.counterparty.fileds.adress +
                  " " +
                  this.counterparty.fileds.telefon,
                style: "tableHeader",
              },
            ],
          },
          { text: " " },
          {
            text: [
              "Основание: ",
              {
                text: "Договор № " + this.contractNumber,
                style: "tableHeader",
              },
            ],
          },
          {
            style: "tableExample",

            table: {
              widths: [20, 290, 50, 50, 50],
              heights: [10, 10, 10, 10, 10],
              body: this.dateTable(),
            },
          },
          {
            columns: [
              " ",
              { text: " " },
              { text: " " },
              {
                type: "none",
                style: "bold",
                ol: [
                  "ндс " + this.allSubTotalNdc(),
                  "Всего " + this.allSubTotal(),
                  " ",
                ],
              },
            ],
          },
          {
            text:
              "Всего наименований " +
              this.items.length +
              " на сумму " +
              this.allSubTotal(),
          },
          { text: " " },
          {
            text: "Оплата данного счета означает согласие с условиями поставки товара. \n Уведомление об оплате обязательно, в противном случае не гарантируется наличие товара на складе.Товар отпускается по факту прихода денег на р/с Поставщика, самовывозом, при наличии доверенности и паспорта.",
          },
          {
            text: "---------------------------------------------------------------------------------------------------------------------------------------------------------",
          },
          { text: " " },
          { text: " " },
          {
            text: "Руководитель _______________________________________________________________________________",
          },
        ],

        styles: {
          header: {
            fontSize: 18,
            bold: true,
            margin: [0, 0, 0, 10],
          },
          subheader: {
            fontSize: 16,
            bold: true,
            margin: [0, 10, 0, 5],
          },
          tableExample: {
            margin: [0, 5, 0, 15],
          },
          tableHeader: {
            bold: true,
            fontSize: 13,
            color: "black",
            margin: [0, 5, 0, 15],
          },
          bold: {
            bold: true,
          },
        },
      };
      pdfMake.createPdf(docDefinition).download("optionalName.pdf");
    },

    dateTable() {
      let dateTab = [["№", "Товары", "Кол-во", "Цена", "Сумма"]];
      for (let i = 0; i < this.items.length; i++) {
        dateTab.push([
          i + 1,
          this.items[i].nomenclature,
          this.items[i].quantity,
          this.items[i].price,
          this.items[i].amount,
        ]);
      }
      return dateTab;
    },

    subtotalNdc(item) {
      return Number(item.ndc.toFixed(2));
    },

    allSubTotal() {
      return this.items.map((item) => item.amount).reduce((a, b) => a + b, 0);
    },

    allSubTotalNdc() {
      return this.items
        .map((item) => this.subtotalNdc(item))
        .reduce((a, b) => a + b, 0);
    },
  },
  mounted() {
    bus.$on("select-items", (datas) => {
      if (datas.listOrgCount === "Organization") {
        this.organization = datas;
      } else {
        this.counterparty = datas;
      }
    });
  },
};
</script>
<style src="./style.css"></style>
<template src="./kulmigrid.html"></template>

<script>
export default {
  data: () => ({
    sTeam1: "DoJo",
    sTeam2: "MaBe",
    colors: ["E", "S", "L", "H", "G", "B", "7", "8", "K"],
    state: {
      date: "",
      team1: {
        player1: "",
        player2: "",
        points: [{ H1: 396 }, { 71: 931 }, { E1: 10 }],
        wies: [{ 71: 420 }]
      },
      team2: {
        player1: "",
        player2: "",
        points: [{ E2: -29 }, { L2: 189 }, { K2: 1233 }],
        wies: [{ E2: 20 }, { L2: 660 }]
      }
    },
    valid: false,
    firstname: "",
    lastname: "",
    selected: "",
    points1: 0,
    points2: 0,
    wies1: 0,
    wies2: 0,
    matsch1: false,
    matsch2: false,
    stoeck1: false,
    stoeck2: false,
    nameRules: [
      v => !!v || "Name is required",
      v => v.length <= 10 || "Name must be less than 10 characters"
    ],
    email: "",
    emailRules: [
      v => !!v || "E-mail is required",
      v => /.+@.+/.test(v) || "E-mail must be valid"
    ],
    pointRules: [
      v => !!v || "",
      v => v >= 0 || "",
      v => v <= 157 || v == 257 || ""
    ],
    wiesRules: [
      v => !v || v == 0 || v >= 20 || "",
      v => !v || v == 0 || v % 10 == 0 || ""
    ]
  }),
  methods: {
    getPointsTeam1: function(color) {
      if (color == "E") {
        return 123;
      }
      return "";
    },
    getPointsTeam2: function(color) {
      if (color == "E") {
        return 123;
      }
      return "";
    },
    getWiesTeam1Color: function(color) {
      if (color == "E") {
        return 123;
      }
      return "";
    },
    getWiesTeam2Color: function(color) {
      if (color == "E") {
        return 123;
      }
      return "";
    },
    getWiesTeam1ForLine: function(color) {
      if (color == "E") {
        return 123;
      }
      return "";
    },
    getWiesTeam2ForLine: function(color) {
      if (color == "E") {
        return 123;
      }
      return "";
    },
    onTableClick: function(color, teamnumber) {
      /* mark the selected field */
      if (this.selected !== "") {
        document.getElementById(this.selected).classList.remove("marked");
      }
      this.selected = color + teamnumber.toString();
      console.log(this.selected);
      console.log(document.getElementById(this.selected));
      document.getElementById(this.selected).classList.add("marked");

      /* load the points into the editor */
    },
    writePoints: function(isTeam1) {
      if (isTeam1) {
        /* validate points */
        if (
          (this.points1 >= 0 && this.points1 <= 157) ||
          this.points1 == 257 ||
          this.points1 == 514
        ) {
          if (this.points1 > 157) {
            this.points2 = 0;
            this.matsch1 = true;
            this.mathsch2 = false;
          } else {
            this.points2 = 157 - this.points1;
            this.matsch1 = false;
          }
        } else {
          this.points1 = 0;
          this.points2 = 0;
          this.matsch1 = false;
          this.matsch2 = false;
        }
      } else {
        /* validate points */
        if (
          (this.points2 >= 0 && this.points2 <= 157) ||
          this.points2 == 257 ||
          this.points2 == 514
        ) {
          if (this.points2 > 157) {
            this.points1 = 0;
            this.matsch2 = true;
            this.matsch1 = false;
          } else {
            this.points1 = 157 - this.points1;
            this.matsch2 = false;
          }
        } else {
          this.points1 = 0;
          this.points2 = 0;
          this.matsch1 = false;
          this.matsch2 = false;
        }
      }
    },
    writeWies(isTeam1) {
        if (isTeam1) {
            if (this.wies1 >= 20 && this.wies1 %10 == 0) {
                this.wies2 = 0;
            } else {
                this.wies1 = 0;
            }
        } else {
            if (this.wies2 >= 20 && this.wies2 %10 == 0) {
                this.wies1 = 0;
            } else {
                this.wies2 = 0;
            }
        }
    },
    matschClicked(isTeam1) {
        console.log("match clicked: " + isTeam1);
        if (isTeam1) {
            if (this.matsch1) {
                this.matsch2 = false;
                if (this.isTeam1Playing()) {
                    this.points1 = 257;
                } else {
                    this.points1 = 514;
                }
                this.points2 = 0;
            }
        } else {
            if (this.matsch2) {
                this.matsch1 = false;
                if (this.isTeam1Playing) {
                    this.points2 = 514;
                } else {
                    this.points2 = 257;
                }
                this.points1 = 0;
            }
        }
    },
    isTeam1Playing() {
        return this.selected.substring(1) == '1';
    }
  },
  mounted() {
      this.onTableClick("E", "1");
  }
};
</script>


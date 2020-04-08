<style src="./style.css"></style>
<template src="./kulmigrid.html"></template>

<script>
export default {
  data: () => ({
    roundCount: 0,
    sTeam1: "DoJo",
    sTeam2: "MaBe",
    colors: ["E", "S", "L", "H", "G", "B", "7", "8", "K"],
    state: {
      date: "",
      team1: {
        player1: "",
        player2: "",
      },
      team2: {
        player1: "",
        player2: "",
      },
      rounds: [{
          round: 1,
          played: 'E1',
          points1: 0,
          points2: 514,
          wies1: 20,
          wies2: 0,
          stoeck1: true,
          stoeck2: false
      },
      {
          round: 2,
          played: 'S1',
          points1: 0,
          points2: 514,
          wies1: 20,
          wies2: 0,
          stoeck1: true,
          stoeck2: false
      },
      {
          round: 3,
          played: 'L1',
          points1: 0,
          points2: 514,
          wies1: 20,
          wies2: 0,
          stoeck1: true,
          stoeck2: false
      },
      {
          round: 4,
          played: 'H1',
          points1: 0,
          points2: 514,
          wies1: 20,
          wies2: 0,
          stoeck1: true,
          stoeck2: false
      },
      {
          round: 5,
          played: 'G1',
          points1: 0,
          points2: 514,
          wies1: 20,
          wies2: 0,
          stoeck1: true,
          stoeck2: false
      },
      {
          round: 6,
          played: 'B1',
          points1: 0,
          points2: 514,
          wies1: 20,
          wies2: 0,
          stoeck1: true,
          stoeck2: false
      },
      {
          round: 7,
          played: '71',
          points1: 157,
          points2: 0,
          wies1: 20,
          wies2: 0,
          stoeck1: true,
          stoeck2: false
      },
      {
          round: 8,
          played: '81',
          points1: 0,
          points2: 514,
          wies1: 20,
          wies2: 0,
          stoeck1: true,
          stoeck2: false
      },
      {
          round: 9,
          played: 'K1',
          points1: 0,
          points2: 514,
          wies1: 20,
          wies2: 0,
          stoeck1: true,
          stoeck2: false
      },
      {
          round: 10,
          played: 'S2',
          points1: 0,
          points2: 257,
          wies1: 20,
          wies2: 0,
          stoeck1: true,
          stoeck2: false
      }]
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
        var points = 0;
        var filtered = this.state.rounds.find(r => r.played === color +"1");
        if (filtered) {
            points = filtered.points1 - filtered.points2;
            points *= this.pointMultiplicator(color);
        }
        if (points == 0) return "";
        return points;
    },
    getPointsTeam2: function(color) {
      var points = 0;
        var filtered = this.state.rounds.find(r => r.played === color +"2");
        if (filtered) {
            points = filtered.points2 - filtered.points1;
            points *= this.pointMultiplicator(color);
        }
        if (points == 0) return "";
        return points;
    },
    getWiesTeam1ForLine: function(color) {
      var validWiesRounds = this.state.rounds.filter(r => (r.wies1 > 0 && !(r.points2 > "157" )) || r.stoeck1);
      var wiesNumbers = validWiesRounds.map(r => {
          var points = 0;
          if (r.stoeck1) {
              points = 20;
          } 
          if (r.points2 <= 157) {
              points += r.wies1;
          }
          return points * this.pointMultiplicator(r.played);
      });
      /* add exeeding number of wies at the beginning */
      if (wiesNumbers.length > 9) {
          for (var i = 9; i < wiesNumbers.length; i++) {
              wiesNumbers[i%9] += wiesNumbers[i];
          }
      }
      var index = this.pointMultiplicator(color) -1;
      if (index < wiesNumbers.length) {
          return wiesNumbers[index];
      } 
      return "";
    },
    getWiesTeam2ForLine: function(color) {
      var validWiesRounds = this.state.rounds.filter(r => (r.wies2 > 0 && !(r.points1 > "157" )) || r.stoeck2);
      var wiesNumbers = validWiesRounds.map(r => {
          var points = 0;
          if (r.stoeck2) {
              points = 20;
          } 
          if (r.points1 <= 157) {
              points += r.wies2;
          }
          return points * this.pointMultiplicator(r.played);
      });
      /* add exeeding number of wies at the beginning */
      if (wiesNumbers.length > 9) {
          for (var i = 9; i < wiesNumbers.length; i++) {
              wiesNumbers[i%9] += wiesNumbers[i];
          }
      }
      var index = this.pointMultiplicator(color) -1;
      if (index < wiesNumbers.length) {
          return wiesNumbers[index];
      } 
      return "";
    },
    onTableClick: function(color, teamnumber) {
      /* mark the selected field */
      if (this.selected !== "") {
        document.getElementById(this.selected).classList.remove("marked");
      }
      this.selected = color + teamnumber.toString();
      document.getElementById(this.selected).classList.add("marked");

      /* load the points into the editor */
      var playedRound = this.state.rounds.filter(r => r.played == color + teamnumber.toString());
      
      if (playedRound.length == 1) {
            var r = playedRound[0];
            this.points1 = r.points1;
            this.points2 = r.points2;
            this.wies1 = r.wies1;
            this.wies2 = r.wies2;
            if (r.points1 > 157) {
                this.matsch1 = true;
            } else {
                this.matsch1 = false;
            }
            if (r.points2 > 157) {
                this.matsch2 = true;
            } else {
                this.matsch2 = false;
            }
            this.stoeck1 = r.stoeck1;
            this.stoeck2 = r.stoeck2;
      } else {
          this.points1 = 0;
          this.points2 = 0;
          this.wies1 = 0;
          this.wies2 = 0;
          this.matsch1 = false;
          this.matsch2 = false;
          this.stoeck1 = false;
          this.stoeck2 = false;
      }
    },
    onWritePoints: function(isTeam1) {
        this.points1 = parseInt(this.points1);
        this.points2 = parseInt(this.points2);
      if (isTeam1) {
        /* validate points */
        if (
          (this.points1 >= 0 && this.points1 <= 157) ||
          (this.points1 == 257 && this.isTeam1Playing()) ||
          (this.points1 == 514 && !this.isTeam1Playing())
        ) {
          if (this.points1 > 157) {
            this.points2 = 0;
            this.matsch1 = true;
            this.matsch2 = false;
          } else {
            this.points2 = 157 - this.points1;
            this.matsch1 = false;
            this.matsch2 = false;
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
          (this.points2 == 257 && !this.isTeam1Playing()) ||
          (this.points2 == 514 && this.isTeam1Playing()) 
        ) {
          if (this.points2 > 157) {
            this.points1 = 0;
            this.matsch2 = true;
            this.matsch1 = false;
          } else {
            this.points1 = 157 - this.points2;
            this.matsch2 = false;
            this.matsch1 = false;
          }
        } else {
          this.points1 = 0;
          this.points2 = 0;
          this.matsch1 = false;
          this.matsch2 = false;
        }
      }
    },
    onWriteWies(isTeam1) {
        this.wies1 = parseInt(this.wies1);
        this.wies2 = parseInt(this.wies2);

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
    onMatschClicked(isTeam1) {
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
    onStoeckClicked(isTeam1) {
        if (isTeam1) {
            if (this.stoeck1) {
                this.stoeck2 = false;
            }
        } else {
            if (this.stoeck2) {
                this.stoeck1 = false;
            }
        }
    },
    isTeam1Playing() {
        return this.selected.substring(1) == '1';
    },
    isCurrentStateValid() {
        if (this.points1 < 0 || this.points2 < 0) return false;
        
        /* match cases */
        if (this.isTeam1Playing()) {
            if (this.points1 == 514 || this.points2 == 257) return false;
        } else {
            if (this.points1 == 257 || this.points2 == 514) return false;
        }

        if (this.matsch1 && this.matsch2) return false;

        if (!(this.matsch1 || this.matsch2)) {
            if (this.points1 > 157 || this.points2 > 157) return false;
            if ((this.points1 + this.points2) != 157) return false;
        }

        /* validate wies */
        if (this.wies1 < 0 || this.wies2 < 0) return false;
        if (this.wies1 > 0 && this.wies2 > 0) return false;
        if ((this.wies1 + this.wies2) %10 != 0) return false;
        if (this.wies1 > 0 && this.wies1 < 20) return false;
        if (this.wies2 > 0 && this.wies2 <20) return false;

        if (this.stoeck1 && this.stoeck2) return false;
        console.log('valid');
        return true;
    },
    pointMultiplicator(colorOrPlayed) {
        return this.colors.indexOf(colorOrPlayed.substring(0,1)) + 1;
    },
    deleteRound() {
        console.log(this.state.rounds);
        this.state.rounds = this.state.rounds.filter(r => r.played != this.selected);
        this.points1 = 0;
        this.points2 = 0;
        this.matsch1 = false;
        this.matsch2 = false;
        this.wies1 = 0;
        this.wies2 = 0;
        this.stoeck1 = 0;
        this.stoeck2 = 0;
        console.log(this.state.rounds);
    },
    goBack() {
        var highestRoundNumber = this.getHighestRoundNumber();
        this.state.rounds = this.state.rounds.filter(r => r.round != highestRoundNumber);
        this.roundCount = this.getHighestRoundNumber();
    },
    getHighestRoundNumber() {
        var roundNumbers = this.state.rounds.map(r => r.round);
        if (roundNumbers.length != 0) {
            return roundNumbers.reduce((a, b) => Math.max(a,b));
        }
        return 0;
    },
    enterRound() {

        if (this.matsch1) {
            this.wies2 = 0;
        }
        if (this.matsch2) {
            this.wies1 = 0;
        }
        
        if (this.isCurrentStateValid()) {
            this.roundCount += 1;
            var alreadyPlayed = this.state.rounds.find(r => r.played == this.selected);

            if (alreadyPlayed) {
                alreadyPlayed.round = this.roundCount;
                alreadyPlayed.points1 = this.points1;
                alreadyPlayed.points2 = this.points2;
                alreadyPlayed.wies1 = this.wies1;
                alreadyPlayed.wies2 = this.wies2;
                alreadyPlayed.matsch1 = this.matsch1;
                alreadyPlayed.matsch2 = this.matsch2;
                alreadyPlayed.stoeck1 = this.stoeck1;
                alreadyPlayed.stoeck2 = this.stoeck2;
            } else {
                this.state.rounds.push({
                round: this.roundCount,
                played: this.selected,
                points1: this.points1,
                points2: this.points2,
                wies1: this.wies1,
                wies2: this.wies2,
                stoeck1: this.stoeck1,
                stoeck2: this.stoeck2
                });
            }
        }
    }, 
    selectedWithTeams() {
        console.log(this.selected);
        if (this.selected.substring(1) == 1) return this.selected.substring(0,1) + ' ' + this.sTeam1;
        return this.selected.substring(0,1) + ' ' + this.sTeam2;
    }
  },
  mounted() {
      /* select the default field */
      this.onTableClick("E", "1");

      /* set the round counter if there is already a jass in the state */
      this.roundCount = this.getHighestRoundNumber();
      
  }
};
</script>


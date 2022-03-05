<style src="./style.css"></style>
<template src="./landingScreen.html"></template>

<script>
export default {
    data: () => ({
        jaesse: [],
        player1: '',
        player2: '',
        player3: '',
        player4: '',
        sTeam1: '',
        sTeam2: ''
    }),
    methods: {
        startJass() {
            if (this.player1 !== '' && this.player2 !== '' && this.player3 !== '' && this.player4 !== '') {
                
                this.sTeam1 = this.player1.substring(0,2) + this.player2.substring(0,2);
                this.sTeam2 = this.player3.substring(0,2) + this.player4.substring(0,2)
                var newId = this.getDate() + "_" + this.sTeam1 + "_vs_" + this.sTeam2;
                console.log(newId);
                this.jaesse.push(newId);
                localStorage.setItem('jaesse', JSON.stringify(this.jaesse));
                
                this.$router.push({name: 'jass', params: 
                    {
                        jassId: newId, 
                        player1: this.player1,
                        player2: this.player2,
                        player3: this.player3,
                        player4: this.player4,
                        sTeam1: this.sTeam1,
                        sTeam2: this.sTeam2
                    }});
            }

        },
        getDate() {
            var date = new Date();
            var dateString = date.getFullYear() + '-' + (date.getMonth()+1) + '-' + date.getDate() + "_" + date.getHours() + ":" + date.getMinutes();
            return dateString;
        },
        openJass(jass) {
            this.$router.push({name: 'jass', params: {jassId: jass}});
        },
        playRevanche(oParams) {
            var newId = this.getDate() + "_" + oParams.sTeam1 + "_vs_" + oParams.sTeam2;
            console.log(newId);
            this.jaesse.push(newId);
            localStorage.setItem('jaesse', JSON.stringify(this.jaesse));
            
            this.$router.push({name: 'jass', params: 
                {
                    jassId: newId, 
                    player1: oParams.player1,
                    player2: oParams.player2,
                    player3: oParams.player3,
                    player4: oParams.player4,
                    sTeam1: oParams.sTeam1,
                    sTeam2: oParams.sTeam2
                }});
        }
    },
    mounted() {
        var storedKeys = localStorage.getItem('jaesse');
        if (storedKeys) {
            try {
                var jaesse = JSON.parse(localStorage.getItem('jaesse'));
                jaesse.sort((a, b) => b.localeCompare(a));
                this.jaesse = jaesse;
            } catch(e) {
                localStorage.removeItem('jaesse');
            }
        }
        if (this.$route.name == 'revanche') {
            this.playRevanche(this.$route.params);
        }
    }
};
</script>


<template>
    <div class="chifoumi">
        <div class="containbattle">
             <div class="title">
            <div class="ttre">Chifoumi</div>
            <div class="hud">
                <div class="point">
                    Score : {{PointJoueur}}
                </div>
                <div class="round">
                     Round : {{Round}}
                </div>
            </div>
           
            
        </div>
        <div class="battleScene">
            <div v-if="JoueurGagne != null" class="result">{{Result}}</div>
            <!-- <div class="countdown">{{Countdown}}</div> -->
            <div class="joureurScene">
                Votre choix :
                <div class="blocarene" :class="AttackChoisie">{{AttackChoisie}}</div>
            </div>
            <div class="enemyScene">
                Choix de l'adversaire : 
                <div class="blocarene" :class="EnemyChoice">{{EnemyChoice}}</div>
            </div>
        </div>
        <div class="chooseAttack">
            <div v-for="Attack in Attacks" v-bind:key="Attack.index">
                <div class="blochoix" :class="Attack" @click="ChoixAttack($event)" v-if="AttackPossible">
                    {{Attack}}
                </div>
            </div>
             <button v-if="JoueurGagne != null" @click.prevent="Rejouer">Rejouer</button>
        </div>
       
        </div>
       
    </div>

</template>

<script>
    export default {
        name: 'Chifoumi',
        data: function () {
            return {
                AttackPossible: true,
                IsFighting: false,
                AttackChoisie: '',
                Round: 1,
                Attacks: ["pierre", "feuille", "ciseaux"],
                Countdown: 3,
                interval: null,
                EnemyChoice: '',
                JoueurGagne: null,
                Resultat: ["Perdu :(", "Gagné !", "Egalité"],
                Result: '',
                PointJoueur: 0

            }
        },
        watch: {

        },
        methods: {
            ChoixAttack: function (e) {
                this.AttackChoisie = e.target.innerHTML
                this.AttackPossible = false
                this.IsFighting = true
                this.Fight()
            },
            Fight: function () {
                if (this.IsFighting) {
                    /* console.log('Fight')
                    if(this.Countdown > 0){
                        var fightCounter = this
                        this.interval = setInterval(function(){
                        fightCounter.Countdown --
                        }, 1000)
                    }else{
                        clearInterval(this.interval)
                    } */
                    this.EnemyChoice = this.Attacks[Math.floor(Math.random() * this.Attacks.length)]
                    switch (this.AttackChoisie) {
                        case "pierre":
                            if (this.EnemyChoice == "ciseaux") {
                                this.JoueurGagne = 1
                            } else if (this.EnemyChoice == "pierre") {
                                this.JoueurGagne = 2
                            } else {
                                this.JoueurGagne = 0
                            }
                            break
                        case "feuille":
                            if (this.EnemyChoice == "pierre") {
                                this.JoueurGagne = 1
                            } else if (this.EnemyChoice == "feuille") {
                                this.JoueurGagne = 2
                            } else {
                                this.JoueurGagne = 0
                            }
                            break
                        case "ciseaux":
                            if (this.EnemyChoice == "feuille") {
                                this.JoueurGagne = 1
                            } else if (this.EnemyChoice == "ciseaux") {
                                this.JoueurGagne = 2
                            } else {
                                this.JoueurGagne = 0
                            }
                            break
                    }
                    if (this.JoueurGagne == 1) {
                        this.PointJoueur++
                    }
                    this.Result = this.Resultat[this.JoueurGagne]
                }


            },
            Rejouer: function () {
                this.AttackPossible = true
                this.IsFighting = false
                this.AttackChoisie = ''
                this.EnemyChoice = ''
                this.JoueurGagne = null
                this.Round ++
            }

        }

    }
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Fredoka+One&display=swap');
    .chifoumi {
         background-image: linear-gradient(to right top, #ffffff, #e9f5ff, #bbf0ff, #78eeff, #12ebeb);
         padding: 100px 0;
         font-family: 'Fredoka One', cursive;
         color: #0a1c23;
    }
    .containbattle{
        width: 500px;
        margin: auto;
        background: #fff;
        box-shadow: 24px 24px 48px #9bbae1, inset -12px -8px 16px #c1d0f0, inset 12px 8px 16px #e1edff;
        padding: 50px;
        border-radius: 8%; 
    }
    .ttre{
        font-size: 2.5rem;
        color: #0785be;
        margin: 25px auto;
    }
    .hud{
        display: flex;
        justify-content: space-between;
        background: #bbf0ff;
        padding: 10px 20px;
        border-radius: 30px;
        margin-bottom: 25px;
    }
    .chooseAttack, .battleScene{
        display: flex;
        justify-content: center;
    }
    .chooseAttack{
        min-height: 150px;
    }
    .blochoix{
        height:100px;
        width: 100px;
        background-size: cover;
        cursor: pointer;
        box-shadow: 7px 10px 48px #9bbae1, inset -12px -8px 16px #c1d0f0, inset 12px 8px 16px #e1edff;
        border-radius: 20px;
        display: flex;
        align-items: flex-end;
        justify-content: center;
        padding: 10px;
        margin:10px;
        transition: 0.2s ease-in;
    }
    .blochoix:hover{
        transform: translateY(-10px);
        box-shadow: 7px 14px 48px #9bbae1, inset -12px -8px 16px #c1d0f0, inset 12px 8px 16px #e1edff;
    }
    .blocarene{
        width:225px;
        height: 225px;
        position: relative;
    }
    .joureurScene{
        border-right: 1px solid #9bbae1;
       
    }
    .pierre{
        background: url(../assets/chifoumi/rocks.png) no-repeat center center;
        background-size: 50%;
    }
    .feuille{
        background: url(../assets/chifoumi/paper.png) no-repeat center center;
        background-size: 50%;
    }
    .ciseaux{
        background: url(../assets/chifoumi/scissors.png) no-repeat center center;
        background-size: 50%;
    }
    .result{
        position: absolute;
        font-size: 2rem;
        text-align: center;
        width: 100%;
        color: rgb(131, 10, 10);
        top:45%;
    }
    button{
        background: #0785be;
        color: #fff;
        font-size: 1.2rem;
        padding: 10px 16px;
        border-radius: 30px;
        border: none;
        box-shadow: 7px 14px 48px #9bbae1
    }
</style>
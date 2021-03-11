<template>
<div class="block">
  <div class="container">
    <div class="card mt-4" v-if="!mevcutSoru">
      <div class="card-header text-center">
        <h4 class="basla_header"> {{tamamlandi ? `Yarışı ${puan} xal ilə bitirdiniz` : 'Yarışa başlamaq üçün buttona basın'}}
      </h4>
      </div>
      <div class="card-header">
        <button class="basla_button btn btn-light" @click="basla()">{{!tamamlandi ? 'Başla' : 'Yenidən başla'}}</button>
      </div>
    </div>
    <div class="card mt-4" v-else>
      <div class="card-header">
          <h5>{{mevcutSoru.soru}} ?</h5>
      </div>
      <div class="card-body">
          <div class="harfler d-flex">
            <div class="harf shadow mr-3" v-for="(harf,index) in harfler" :key="'harf-'+index">
            <span v-if="harf.acildi">{{harf.harf}}</span>
            <span v-else></span>
            </div>
          </div>
      </div>
      <div class="card-footer d-flex">
        <h5 class="xal">Hərf xalı : {{harfPuan}}</h5>
        <h5 class="xal"> Qalan vaxt : {{ kalanSure }}</h5>
         <h5 class="xal">Bütün xal : {{puan}}</h5>
      </div>
      <div class="card-footer">
        <input class="form-control" type="text" placeholder="cavabı yazın" v-model="yarismaciCevap" />
        <p class="mt-3">Cavabınız : {{yarismaciCevap}}</p>
        <button class="btn btn-success" @click="cevapver()">Cavab ver</button>
        <button class="btn btn-secondary" @click="harfver">Hərf ver</button>
      </div>
    </div>
  </div>
  </div>
</template>


<script>
export default {
  data(){
    return{
      sorular : [
        {
          soru : "Azərbaycan Respublikasının ən böyük şəhəri",
          cevap : "Bakı",
          soruldu : false
        },
        {
          soru : "Azərbaycan Respublikasının paytaxtı",
          cevap : "Bakı",
          soruldu : false
        }

      ],
      mevcutSoru : null,
      harfler : [],
      puan : 0,
      harfPuan: 0,
      yarismaciCevap : "",
      tamamlandi : false,
      sure : null,
      kalanSure : 0
    }
  },
  methods : {
    basla(){
      this.mevcutSoru = null
    this.sorular.map(x=>{
      x.soruldu = false
    })
    this.puan = 0
    this.kalanSure = this.sorular.length*60
    this.sure = setInterval(()=>{
      this.kalanSure--
      if(this.kalanSure === 0){
        this.bitir()
      }
      },1000)
    
      this.soruver()
    },
    bitir(){
      this.tamamlandi = true
      this.mevcutSoru = null
      clearInterval(this.sure)
    },
    soruver(){
      this.mevcutSoru = this.sorular.find(x => !x.soruldu)
      if(!this.mevcutSoru){
        this.bitir()
        return
      }
      this.harfler = []
      this.mevcutSoru.cevap.split('').map(x=>{
        this.harfler.push({
          harf : x,
          acildi : false 
        })
        this.harfPuan = this.harfler.length*100
        this.mevcutSoru.soruldu = true
      })
    },
    harfver(){
     
     let rastgeleHarfIndex = Math.floor(Math.random()*this.harfler.length)
     if(this.harfPuan<=100){
        return;
      }
     let harf = this.harfler[rastgeleHarfIndex]
      
      while(harf.acildi){
         rastgeleHarfIndex = Math.floor(Math.random()*this.harfler.length)
         harf = this.harfler[rastgeleHarfIndex]
      }
      harf.acildi = true
      this.harfPuan = this.harfPuan-100
    },
    cevapver(){
      if(!this.yarismaciCevap){
        return
      }
      let cevap = this.yarismaciCevap.toLocaleUpperCase('TR')
      this.yarismaciCevap = cevap
      if(this.yarismaciCevap === this.mevcutSoru.cevap.toLocaleUpperCase('TR')){
        this.puan +=this.harfPuan
      }
      else{
        this.puan -=this.harfPuan
      }
      this.yarismaciCevap = ""
      this.soruver()
    }
  }
}
</script>

.<style>
.harf{
  width: 100px;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 30px;
}

.block{
  background-color: #14bdac;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.basla_header{
  color: #0da192;
}

.basla_button{
  width: 100%;
  border : 2px solid #14bdac;
  color: #0da192;
  font-size: 20px;
  font-weight: 600;

}

.basla_button:hover{
  background-color: #14bdac;
  color: #fff;
  transition: 1s;
}

.xal{
  font-size: 16px;
  margin-right: 20px;
  margin-bottom: 0;
  font-weight: 400;
}

</style>
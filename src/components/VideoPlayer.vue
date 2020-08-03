<template>
    <div class="video-player">
        <div class="video-container">
            <iframe width="640" height="360" :src="this.activeVideo.youtubeURL" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
            <h3>{{this.activeVideo.title}}</h3>
            <div class="row">
                <p>{{ this.activeVideo.views }} visualizações</p> 
                <p>{{ this.activeVideo.likes }} <button @click="addLike">Curtir</button></p>
            </div>
            <textarea name="comentarios" id="comentarios" cols="80" rows="20" placeholder="Comente aqui ..."></textarea>
            <button>Comentar</button>
        </div>
        <div class="video-list">
            <input type="search" class="filtro" @input="filtro = $event.target.value" placeholder="Pesquisar ...">
            <div @click="chooseVideo(video)" :key="video.id" v-for="video in videosComFiltro" class="thumbnail">
                <div class="thumbnail-img">
                    <img :src="video.thumbnail" />
                </div>
                <div class="thumbnail-info">
                    <h3>{{video.title}}</h3>
                    <p>{{video.creator}}</p>
                    <p class="thumbnail-views">{{ video.views }} visualizações</p>
                </div>
            </div>
        </div>

    </div>
</template>

<script>

    let videos = [
        {
            id: 1,
            title: "Jaleko, o melhor amigo do estudante de medicina!",
            thumbnail: "https://i.ytimg.com/an_webp/c_djjYIw3Ps/mqdefault_6s.webp?du=3000&sqp=CKT4nfkF&rs=AOn4CLCb3h1H12y3DMk4XA20qilbIhgEiQ",
            youtubeURL: "https://www.youtube.com/embed/c_djjYIw3Ps",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 2,
            title: "Conheça a nova plataforma Jaleko!",
            thumbnail: "https://i.ytimg.com/an_webp/hQcwm02vWqo/mqdefault_6s.webp?du=3000&sqp=CPaJnvkF&rs=AOn4CLBtyiV3w_p3ZXEZX7aYxZRuoLAejQ",
            youtubeURL: "https://www.youtube.com/embed/hQcwm02vWqo",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 3,
            title: "5 lesões comuns em esportes populares - Listas Jaleko #04",
            thumbnail: "https://i.ytimg.com/vi/-LA8htAdsqQ/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLC5Au8mStm8jPhKMPjqY054dwrK9w",
            youtubeURL: "https://www.youtube.com/embed/-LA8htAdsqQ",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 4,
            title: "5 nomes anatômicos mais BIZARROS - Listas Jaleko #03",
            thumbnail: "https://i.ytimg.com/vi/AOMqFp2r0mE/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLAeraB7UefGolBxfXjTOkHSuZGkdA",
            youtubeURL: "https://www.youtube.com/embed/AOMqFp2r0mE",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 5,
            title: "5 melhores livros de anatomia! - Listas Jaleko #02",
            thumbnail: "https://i.ytimg.com/vi/os6KfEpozSk/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLCFqqNbLHQsIJ-aae_EN9GrePA_1g",
            youtubeURL: "https://www.youtube.com/embed/os6KfEpozSk",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 6,
            title: "Anatomia cardiovascular: Pericárdio",
            thumbnail: "https://i.ytimg.com/vi/XDdaaIziT2Y/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLBDcKZn7B5THV9v4c4fXTwXrwjx2w",
            youtubeURL: "https://www.youtube.com/embed/XDdaaIziT2Y",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 7,
            title: "Anatomia do sistema digestório: Estômago",
            thumbnail: "https://i.ytimg.com/vi/sn_VZDrz3m0/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLAVfsyQGe7MPKy5co-KtkupH3rNDA",
            youtubeURL: "https://www.youtube.com/embed/sn_VZDrz3m0",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 8,
            title: "Como faço para escolher a minha especialidade médica? - Jaleko Responde #04",
            thumbnail: "https://i.ytimg.com/vi/DYb5s2TH7bw/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLDPGjk4ZsOnBMDrTsHbCECWYf-2fg",
            youtubeURL: "https://www.youtube.com/embed/DYb5s2TH7bw",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 9,
            title: "Depoimento Aluno Jaleko - EP #1",
            thumbnail: "https://i.ytimg.com/vi/a6tYK2KVJ6E/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLBajEWApKQhtMN2_JmhoCoTk3W4WA",
            youtubeURL: "https://www.youtube.com/embed/a6tYK2KVJ6E",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 10,
            title: "5 hipóteses diagnósticas que você pode lançar em toda sessão clínica! - Listas Jaleko #01",
            thumbnail: "https://i.ytimg.com/vi/Lpw7O7PZkxs/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLCbNUGdiMUNxLz5lCdca3j-8qvAZA",
            youtubeURL: "https://www.youtube.com/embed/Lpw7O7PZkxs",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 11,
            title: "Afinal, o que é ECMO? Aprenda tudo que precisa saber com prof. Yuri Albuquerque",
            thumbnail: "https://i.ytimg.com/vi/JUY3h69JYjc/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLDadvpQnYY9Z5DB2x20yQv6dXQ5pg",
            youtubeURL: "https://www.youtube.com/embed/JUY3h69JYjc",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 12,
            title: "Pediatria: Semiologia Pediátrica - Aspectos Gerais",
            thumbnail: "https://i.ytimg.com/vi/TTydeFD5u5E/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLDKeW7-66dMPofs4YhC5tDaDaf9iA",
            youtubeURL: "https://www.youtube.com/embed/TTydeFD5u5E",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 13,
            title: "Imunologia dos tumores com Fabrício Montalvão",
            thumbnail: "https://i.ytimg.com/vi/0cGsVfGNjAo/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLDwl4liwXYteyAn8JKK49NER_ESvw",
            youtubeURL: "https://www.youtube.com/embed/0cGsVfGNjAo",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 14,
            title: "Comemoração do Dia do Calouro - Guia de Sobrevivência na Faculdade de Medicina",
            thumbnail: "https://i.ytimg.com/vi/Ak9INS5ytos/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLCfALwKyhHbzVILyytpbNSOSqDkgA",
            youtubeURL: "https://www.youtube.com/embed/Ak9INS5ytos",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 15,
            title: "O câncer na era do COVID 19 com Fabrício Montalvão e João Viola",
            thumbnail: "https://i.ytimg.com/vi/54_iUVNPAQ4/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLBQMOnM-0vpub1m1pxNcTIT9KJSYA",
            youtubeURL: "https://www.youtube.com/embed/54_iUVNPAQ4",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 16,
            title: "Emergência Sob Controle - Cetoacidose Diabética: Tratamento (Parceria SIMM)",
            thumbnail: "https://i.ytimg.com/vi/bC3HKRVqQI8/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLBVdfNL7bUtmt8BUdtI-Nfcnd7-qQ",
            youtubeURL: "https://www.youtube.com/embed/bC3HKRVqQI8",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 17,
            title: "Síndrome Parkinsoniana: da neuroanatomia aos conceitos clínicos com Lucas Campos e Júlio Santos",
            thumbnail: "https://i.ytimg.com/vi/ImI1BlzaIYU/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLCQOyiXxvLD9G976_udvsJopIcr7g",
            youtubeURL: "https://www.youtube.com/embed/ImI1BlzaIYU",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 18,
            title: "Doenças infecciosas no cenário brasileiro: Uma visão da virologia com destaque ao HIV e Zika.",
            thumbnail: "https://i.ytimg.com/vi/54T0YQtmJhg/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLBFQf-Yn3RjtjWLKeSJhQV3mOJkCg",
            youtubeURL: "https://www.youtube.com/embed/54T0YQtmJhg",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 19,
            title: "#DiaDoHospital - Uma homenagem do Jaleko aos profissionais de saúde",
            thumbnail: "https://i.ytimg.com/vi/yDHX0vML5VQ/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLB3NyxHMZa-texHVsILYg6qbgMRdQ",
            youtubeURL: "https://www.youtube.com/embed/yDHX0vML5VQ",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
        {
            id: 20,
            title: "Oncologia - Introdução ao Câncer de Testículo",
            thumbnail: "https://i.ytimg.com/vi/2u1bqiNWWVk/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLCF5Vm768-Uo4qE7_BxLmEJRb3xSQ",
            youtubeURL: "https://www.youtube.com/embed/2u1bqiNWWVk",
            creator: "Jaleko Acadêmicos",
            likes: 0,
            views: 0
        },
    ];

    export default {
    name: 'VideoPlayer',
    data () {
        return {
            videos,
            activeVideo: videos[0],
            filtro: ''
        }
    },

    computed: {
    videosComFiltro() {
      if (this.filtro) {
        let exp = new RegExp(this.filtro.trim(), 'i');
        return this.videos.filter(video => exp.test(video.title));
      } else {
        return this.videos;
        }
      }
    },

     methods:{
        chooseVideo(video){
            this.activeVideo = video;
            video.views += 1;
        },

        addLike() {
            this.activeVideo.likes += 1;
        }
    }
}

   
</script>

<style scoped>
  .thumbnail{
    display:flex;
    cursor: pointer;
  }

  .thumbnail img{
    width:168px;
}

.thumbnail-info{
    margin-left:20px;
    text-decoration-color: blue;
    cursor: text;
}

.thumbnail h3{
    font-size:16px;
}

h3,
p{
    margin:0;
    padding:0;
}

.thumbnail-views{
    font-size:14px;
}

.video-player{
    display:flex;
    width:1200px;
    margin:auto;
}

.video-container{
    margin-right:40px;
}

.row{
    display:flex;
    justify-content:space-between;
}

button{
    margin-left: 10px;
    background:#D0021B;
    color:white;
    border-radius: 5px;
    padding:10px 20px;
    cursor: pointer;
}

.filtro {
    padding: 10px;
    margin-bottom: 10px;
    display: inline-block;
    width: 95%;
    
}

#comentarios {
    margin: 10px;
}
</style>
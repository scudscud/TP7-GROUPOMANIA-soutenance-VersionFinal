<template>
  <v-card class="card">
    <v-card-text class="card-profil-title">
      <h1 class="card-profil-title-h1">Son profil </h1>
    </v-card-text>

    <v-card-text v-if="url == '' && urlpic == ''" class="card-profiluser">
      <div class="block-picture">
        <label class="lab-pic" for="avatar">
          <div id="avatar-empty-profiluser">{{ avatarpicempty }}</div>

        </label>
      </div>
      <span class="fullname-usermain">{{ fullname }}</span>
    </v-card-text>
    <v-card-text v-else-if=" urlpic !== '' " class="card-profilusermain">
      <div class="block-picture">
        <label class="lab-pic" for="avatar">
          <img id="form-picture-profil" :src="urlpic" alt="phtot de l'utilisateur" />


        </label>
      </div>
      <span class="fullname-profilusermain">{{ fullname }}</span>
    </v-card-text>



    <v-card-text class="card-profil-biographie">
      <h2>biographie </h2>
      <p v-if="this.bioUser == ''" class="card-profil-biographie-p">{{ biographieP }}</p>
      <p v-else class="card-profilusermain-biographie-p">{{ bioUser }}</p>

    </v-card-text>


    <v-card-text class="card-profil-friend">
      <div  v-if="following[0] !== undefined"  class="card-profil-friend-t">
        <v-icon class="icon-friend">mdi-account-group</v-icon>
        <h2 class="h2-friend">Les abonnements de {{ fullname }}&nbsp({{infoAbo.length}})</h2>
      </div>
      <div v-else class="card-profil-friend-t">
        <v-icon class="icon-friend">mdi-account-group</v-icon>
        <h2 class="h2-friend">Les abonnements de {{ fullname }} </h2>
      </div>
      <div v-for="(pi, index) in infoAbo" class="btn-profil-follow">
        <p v-if="pi._id.includes(userjwtid) "  class="card-profiluser-friend">{{fullname }} est un(e) de vos abonné(e) </p>
        <p v-else  class="card-profil-friend-p">{{pi.firstname +" "+pi.lastname }} </p>
        <button :key="followkey " v-if="pi._id.includes(userjwtid) && followBack == false && follower[0] !== undefined" class="btn-followback-profil-user " @click="getFollowBack(userid)"> S'abonné </button>       
      </div>
          <p  v-if="following[0] == undefined"  class="card-profil-friend-p">{{fullname}} ne connait plus personne en Harley Davidson!!  </p>
    </v-card-text>

    <v-card-text  v-if="follower[0] != undefined"  class="card-profil-friend">
      <div  class="card-profil-friend-t">
        <v-icon class="icon-friend">mdi-account-group</v-icon>
        <h2 class="h2-friend">Les abonné(e)s de {{ fullname }}&nbsp({{info.length}})</h2>
      </div>
      <div v-for="(p, index) in info"  class="btn-profil-follow">
        <p v-if="p._id !== userjwtid" class="card-profil-friend-p">{{p.firstname +" "+p.lastname }} </p>
        <p v-if="p._id === userjwtid" class="card-profiluser-friend">Vous êtes abonné(e) à {{fullname}}</p>
        <button v-if="p._id.includes(userjwtid)" class="btn-unfollow " @click="getUnFollowBack(userid)"> Se désabonné</button>
      </div>

      <div v-if="newfollow && !followBack && following.includes(userjwtid)" class="btn-profil-follow">
        <p class="card-profiluser-friend-new">Abonnez-vous à {{fullname}}</p>
        <button class="btn-followback-profil-user " @click="getFollowBack(userid)"> S'abonné</button>
      </div>
      <div v-if="newfollow && !followBack && !following.includes(userjwtid)" class="btn-profil-follow">
        <p class="card-profiluser-friend-new">Faite le 1er pas Abonnez-vous à {{fullname}}</p>
        <button class="btn-followback-profil-user " @click="getFollowBack(userid)"> S'abonné</button>
      </div>

    </v-card-text>
    <v-card-text v-else class="card-profil-friend">
      <div class="card-profil-friend-t">
        <v-icon class="icon-friend">mdi-account-group</v-icon>
        <h2 class="h2-friend">Les abonné(e)s de {{ fullname }}</h2>
      </div>
      <div class="btn-profil-follow">
        <p class="card-profiluser-friend-new">Soyez le 1er Abonné(e) de {{fullname}}</p>
        <button class="btn-followback-profil-user " @click="getFollowBack(userid)"> S abonné</button>
      </div>
    </v-card-text>

    <v-card-text class="card-profil-post" v-if="pub[0] != undefined">
      <div class="card-profil-friend-pub">
        <v-icon class="icon-post">mdi-newspaper-variant-multiple-outline</v-icon>
        <h2 class="h2-post">Les publications de {{ fullname }}&nbsp({{pub.length}})</h2>
      </div>
      <div class="align-pub-user">
        <div class="profilusermain-post" v-for="(p, index) in pub">

          <p class="card-profil-post-p"> publication&nbsp:&nbsp{{p.date}}</p>
          <div class="like-profilmain-user" v-if="p.likers.length != 0">
            <v-icon class="img-like-profilmain">mdi-thumb-up-outline</v-icon><span>{{p.likers.length}}</span>
          </div>
          <div class="btn-post-profil">
            <button :class="p.likers.includes(userjwtid) ? 'class-btn-like-userprofil' : 'class-btn-unlike-userprofil'"
              @click="clickLike(p._id,index)" class="classlikebtn">
              <v-icon class="img-att-userprofil">mdi-thumb-up-outline</v-icon>
              <p class="text-att-userprofil">Like</p>
            </button>
          </div>

          <div v-if='p.picture !="" ' class="image-card-profil"><img class="card-img-profil" :src="p.picture"
              alt="photo" /></div>
          <div v-if="p.message != ''" class="message-profil"> {{p.message}}</div>
        </div>
      </div>
    </v-card-text>
    <v-card-text class="card-profilusermain-post" v-else>
      <div class="card-profil-friend-pub">
        <v-icon class="icon-post">mdi-newspaper-variant-multiple-outline</v-icon>
        <h2 class="h2-post">Les publications de {{ fullname }}&nbsp({{pub.length}})</h2>
      </div>
      <div class="card-profil-post-p">{{ publication }}</div>
    </v-card-text>
    <div>
      <Load v-show="showloader" @close-modale-loader="showloader = false" @open-modale-loader="true" />
    </div>

  </v-card>

</template>
  
<script>
import axios from "axios";
import Load from "../components/Waitload.vue"

export default {
  name: "Profil",
  components: {
 Load
  },

  data() {
    return {
      id: '',
      url: "",
      urlpic: "",
      avatarpicempty: "",
      picutername: "",
      modifbio: false,
      deletebiouser: false,
      bio: "",
      bioUser: "",
      newBioUser: '',
      biographieP: "C'est vide, cette personne est assez secrete !!! ",
      friend: "Aie c'est vide,cette personne n'as pas d'amis faite le premier pas !!! ",
      friendAbo: "Cette personne ne connait plus personne en Harley Davidson, abonné(e) vous à elle, elle vous le rendras surement ",
      follower: [],
      followBack: false,
      followBackId: [],
      followInfo: [],
      newfollowInfo: [],
      info: [],
      followId: '',
      userjwtid: '',
      myfollower:[],
      myfollowing:[],

      userLikePostId: [],

      newfollow: true,
      following: [],
      followingBack: false,
      followingBackId: [],
      followingInfo: [],
      newfollowingInfo: [],
      followingId: '',
      infoAbo: [],

      publication: "Aie c'est vide, cette personne n'as rien publié",
      publications: [],
      pub: [],
      newpub:[],
      likepub:[],

      lastname: "",
      firstname: "",
      photo: [],
      userid: "",
      posted: "",


      followkey: 0,

      showdel: false,
      showmodify: false,
      warningRecord: false,
      warningEmpty: false,
      warningDelete: false,

      bioValid: false,
      showloader:true,


    };
  },
  computed: {
    fullname: {
      get() {
        return this.firstname + " " + this.lastname;
      },
      set(newValue) {
        [this.firstname, this.lastname] = newValue.split(" ");
      },
    },
  },

  methods: {


    clickLike(postId, index) {
      const likeBtn = document.querySelectorAll('.classlikebtn')
      if (likeBtn[index].classList.contains('class-btn-unlike-userprofil')) {
        axios.patch(`http://localhost:5000/api/post/like-post/${postId}`, { id: this.userjwtid })
          .then(() => {
            axios.get(`http://localhost:5000/api/user/${this.id}`)
              .then((user) => {
                this.userLikePostId = user.data.likes
              }).catch((err) => { console.log(err) })
            likeBtn[index].classList.replace('class-btn-unlike-userprofil', 'class-btn-like-userprofil')
            this.getPosts()
          }).catch((err) => { console.log(err) })
      } else {
        axios.patch(`http://localhost:5000/api/post/unlike-post/${postId}`, { id: this.userjwtid })
          .then(() => {
            axios.get(`http://localhost:5000/api/user/${this.id}`)
              .then((user) => {
                this.userLikePostId = user.data.likes
              }).catch((err) => { console.log(err) })
            likeBtn[index].classList.replace('class-btn-like-userprofil', 'class-btn-unlike-userprofil')
            this.getPosts()
          }).catch((err) => { console.log(err) })
        }
    },

    getcolor() {
      if (this.urlpic === "") {
        this.avatarpicempty = this.lastname.split("")[0].toLocaleUpperCase();
      }
    },

    getImage(imagename) {
      let name = avatar.value;
      let namereg = name.replace(/^.*\\/, "");
      this.picutername = namereg;
    },

    getPosts() { 
      axios.get(`http://localhost:5000/api/post/postby/${this.id}`)
        .then((doc)=>{
          this.pub= doc.data
          console.log(doc.data);
        }).catch((err) => { console.log(err); });
    },

    getFollowBack(id) {
      axios.patch(`http://localhost:5000/api/user/follow/${this.userjwtid}`, { idToFollow: id })
        .then(() => {
          axios
            .get(`http://localhost:5000/api/user/${this.id}`)
            .then((docs) => {
              this.follower = docs.data.followers;
              this.following = docs.data.following;
            })
            .catch((error) => {
              console.log(error); })
            .then(() => {
                axios.get(`http://localhost:5000/api/user/${this.userjwtid}`)
                  .then((docs) => {
                    this.followId = docs.data._id
                    this.followLastname = docs.data.lastname;
                    this.followFirstname = docs.data.firstname;
                    this.followBackId = docs.data.followers;
                    this.followInfo = docs.data
                  
                    if (this.follower.includes(this.userjwtid)) {
                      this.followBack = true
                      this.newfollow = false
                    }
                    this.info.push(this.followInfo)
                  });
              });
            }).catch((error) => {
              console.log(error);
            })
    },


    getUnFollowBack(id) {
      axios.patch(`http://localhost:5000/api/user/unfollow/${this.userjwtid}`, { idToUnFollow: id })
        .then(() => {
          axios
            .get(`http://localhost:5000/api/user/${this.id}`)
            .then((docs) => {
              this.follower = docs.data.followers;
              this.following = docs.data.following;
            })
            .catch((error) => {
              console.log(error);
            })
            .then(() => {
                axios.get(`http://localhost:5000/api/user/${this.userjwtid}`)
                  .then((docs) => {
                    // console.log(docs);
                    this.followId = docs.data._id
                    this.followLastname = docs.data.lastname;
                    this.followFirstname = docs.data.firstname;
                    this.followBackId = docs.data.followers;
                    this.followInfo = docs.data
                    const newarr  = this.info.filter(data=> data._id != this.followId)
                    this.info = newarr
                    
                      if(!this.follower.includes(this.id)){
                        this.followBack = false
                      this.newfollow = true}
                    
                  });
            }).catch((error) => {
              console.log(error);
            })
        })
    },
  },

  async mounted() {
    setTimeout(() => {
      this.showloader = false
    }, 1500);
    axios.defaults.withCredentials = true;
    let params = window.location.toString()
    let idUserURL = params.split('=')[1]
    this.id = idUserURL
    console.log(this.id);
    await axios
      .get(`http://localhost:5000/jwtid`)
      .then((res) => {
        this.userjwtid = res.data;
        this.show = true;
        this.log = true;
      })
      .catch((error) => {
        console.log(error);
      });

    await axios
      .get(`http://localhost:5000/api/user/${this.id}`)
      .then((docs) => {
        this.userid = docs.data._id;
        this.firstname = docs.data.firstname;
        this.lastname = docs.data.lastname;
        this.urlpic = docs.data.photo;
        this.bioUser = docs.data.bio
        this.userLikePostId = docs.data.likes
        this.follower = docs.data.followers;
        this.following = docs.data.following;
      })
      .catch((error) => {
        console.log(error);
      })
      .then(() => {
        this.follower.forEach((i, u, l) => {
          axios.get(`http://localhost:5000/api/user/${i}`)
            .then((docs) => {
              this.followId = docs.data._id
              this.followLastname = docs.data.lastname;
              this.followFirstname = docs.data.firstname;
              this.followBackId = docs.data.followers;
              this.followInfo = docs.data
              this.info.push(this.followInfo)
              if (this.followId === this.userjwtid) {
                this.followBack = true
                this.newfollow = false
              }
              

            });
        });
      }).catch((error) => {
        console.log(error);
      })
      .then(() => {
        this.following.forEach((i) => {

          axios.get(`http://localhost:5000/api/user/${i}`)
            .then((docs) => {
              console.log(docs);
              this.followingId = docs.data._id
              this.followingLastname = docs.data.lastname;
              this.followingFirstname = docs.data.firstname;
              this.followingBackId = docs.data.followers;
              if (this.followingId === this.userjwtid) { this.newfollow = false }
              this.followingInfo = docs.data
              this.infoAbo.push(this.followingInfo)
             
            })
        })
      }).catch((error) => {
        console.log(error);
      })
        axios.get(`http://localhost:5000/api/post/postby/${this.id}`)
        .then((doc)=>{
          this.pub= doc.data
          console.log(doc.data);
        })
    this.getcolor();
  },
};
</script>
  
<style lang="scss">
label.lab-pic {
  display: flex;
  // width: 130px;
}

div.v-card__text.card-profilusermain {
  padding-top: 1%;
  display: flex;
  justify-content: center;
  background-color: $tertiary;
}

div.v-card__text.card-profiluser {
  padding-top: 1%;
  padding-bottom: 2%;
  display: flex;
  justify-content: center;
  background-color: $tertiary;
}

.fullname-profilusermain {
  // padding-top: 1%;
  padding-left: 1%;
  font-size: 1.8rem;
  padding-top: 6%;
}

span.fullname-usermain {
  padding-left: 1%;
  font-size: 1.8rem;
  padding-top: 5%;
}

#avatar-empty-profiluser {
  display: flex;
  width: 100px;
  height: 100px;
  justify-content: center;
  align-items: center;
  border: solid 2px $secondary;
  border-radius: 50%;
  font-size: 5rem;
  padding-bottom: 3%;
  background-color: rgb(6, 132, 6);
}

#form-picture-profil {
  display: flex;
  width: 120px;
  height: 120px;
  justify-content: center;
  align-items: center;
  border: solid 2px $secondary;
  border-radius: 50%;
  font-size: 5rem;
  // padding-bottom: 5%;
}

.card {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.card-profil-title {
  border-bottom: solid 2px $primary;
  background-color: $secondary;
}

.card-profil-title-h1 {
  padding-top: 2%;
  padding-bottom: 1%;
  font-size: 2.5rem;
  font-weight: bolder;
  font-style: italic;
  color: $primary;
}




.lab-pic-custom-url {
  position: relative;
  top: 70px;
  left: 140px;
  height: 38px;
  width: 38px;
  background-color: $tertiary;
  border-radius: 50%;
  border: solid 2px $primary;
  padding-bottom: 2%;
  padding-right: 2%;

  &:hover {
    cursor: pointer;
  }
}

button#btn-confirm-pic-profil {
  display: flex;
  // height: 20px;
  justify-content: center;
  align-items: center;
  margin-left: 0%;
  border: solid 2px $secondary;
  border-radius: 30%;
  color: $secondary;

  &:hover {
    border-radius: 20%;
    background-color: $secondary;
    color: $tertiary;
  }
}

button#btn-confirm-pic-profil-post {
  display: flex;
  // height: 20px;
  justify-content: center;
  align-items: center;
  margin-left: 0%;
  border: solid 2px $secondary;
  border-radius: 30%;
  color: green;

  &:hover {
    border-radius: 20%;
    background-color: $secondary;
    color: green;
  }
}

.form-avatar-profil-url {
  padding-top: 2%;
  display: none;
  visibility: none;

  &:hover {
    cursor: pointer;
  }
}

.card-profil-biographie {
  border-bottom: solid 2px;
  border-bottom: 2px solid $secondary;
  border-top: 2px solid $secondary;
}

p.card-profilusermain-biographie-p {
  margin: 0;
  // padding-left: 1%;
  padding-bottom: 2%;
  // border: 2px solid $secondary;
  cursor: default;
}

.btn-unfollow {
  width: 100px;
  border: solid 2px $secondary;
  // margin-top: 1%;
  // margin-right: 1%;
  // margin-left: 1%;
  border-radius: 30%;
  padding-left: 5px;
  padding-right: 5px;

  &:hover {
    background-color: $secondary;
    color: $tertiary;

    &.btn-unfollow>.pen-icon {
      color: $tertiary;
    }
  }
}
.btn-followback-profil-user {
  width: 100px;
  border: solid 2px rgb(16, 148, 13);
  border-radius: 30%;
  padding-left: 5px;
  padding-right: 5px;
  &:hover {
    background-color: $secondary;
    color: $tertiary;
    &.btn-unfollow>.pen-icon {
      color: $tertiary;
    }
  }
}

.pen-icon {
  padding-bottom: 2%;
}

.card-profil-friend {
  border-bottom: 2px solid $secondary;
}

.card-profil-friend-t {
  display: flex;
  flex-direction: row;

}

.card-profil-friend-pub {
  display: flex;
  flex-direction: row;
  justify-content: center;
}

.icon-friend {
  padding-right: 1%;
}

p.card-profil-friend-p {
  // display: flex;
  // justify-content:center;
  // align-items: center;
  margin: 0;
  // padding-left: 1%;
  // padding-bottom: 2%;
  // border: 2px solid $primary;
  cursor: default;
}

p.card-profiluser-friend {
  font-style: italic;
  font-weight: bold;
  margin: 0;
  color: rgb(40, 207, 40);
  cursor: default;
}

p.card-profiluser-friend-new {
  font-style: italic;
  font-weight: bold;
  margin: 0;
  color: rgb(40, 207, 40);

  cursor: default;
}

.btn-profil-follow {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin-bottom: 1%;
  // justify-content: center;
  align-items: center;
}

.card-profil-post {
  border-bottom: solid 2px;
  border-bottom: 2px solid $secondary;
}

.card-profil-post-t {
  display: flex;
  flex-direction: row;
}

.icon-post {
  padding-right: 1%;
}

.btn-post-profil {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 50%;
}

.btn-post-delete-profil {
  width: 100px;
  border: solid 2px $secondary;
  // margin-top: 1%;
  // margin-right: 1%;
  // margin-left: 1%;
  margin-bottom: 1%;
  border-radius: 30%;
  padding-left: 5px;
  padding-right: 5px;

  &:hover {
    background-color: $secondary;
    color: $tertiary;

    &.btn-post-delete-profil>.pen-icon {
      color: $tertiary;
    }
  }
}

.btn-post-modify-profil {
  width: 100px;
  border: solid 2px $secondary;
  margin-bottom: 1%;
  border-radius: 30%;
  padding-left: 5px;
  padding-right: 5px;

  &:hover {
    background-color: $secondary;
    color: $tertiary;
  }
}

.like-profilmain-user {
  display: flex;
  flex-direction: row;
  margin-bottom: 1%;
  align-items: center;
  justify-content: center;
}

.img-like-profilmain {
  margin-right: 10%;
}

.card-img-profil {
  display: flex;
  justify-content: center;
  align-items: center;
  display: flex;
  object-fit: cover;
  max-height: 300px;
  max-width: 500px;
  min-width: 300px;
  border: solid 2px $secondary;
  border-radius: 2%;
}

.align-pub-user {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}

.image-card-profil {
  display: flex;
  width: 100%;
  height: 100%;
  padding: 1% 1%;
  // border-top: solid 2px $secondary;
  justify-content: center;
  align-items: center;
}

.message-profil {
  padding: 2%;
  margin-top: 1%;

  text-align: center;
  max-width: 500px;
  width: 100%;
  border-top: solid 2px;
  border-radius: 2%;
}

button.class-btn-like-userprofil {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  border-radius: 5%;
  height: 38px;
  margin-top: 2%;
  margin-right: 3%;
  background-color: rgb(27, 108, 17);
  color: $secondary;
  font-style: italic;
  font-weight: bold;
  width: auto;
  cursor: pointer;
  padding: 2%;

  &.class-btn-like-userprofil>.img-att-userprofil:before {
    color: $secondary;
  }

  &:hover {
    background-color: rgb(27, 108, 17);
    color: $secondary;
    translate: 3px;
    border: solid 1px $secondary;
  }
}

p.text-att-userprofil {
  margin-bottom: 0;
}

button.class-btn-unlike-userprofil {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  border-radius: 5%;
  height: 38px;
  margin-top: 2%;
  margin-right: 3%;
  background-color: $secondary;
  color: $tertiary;
  width: auto;
  cursor: pointer;
  padding: 2%;

  &.class-btn-unlike-userprofil>.img-att-userprofil:before {
    color: $tertiary;
  }

  &:hover {
    background-color: $tertiary;
    color: $secondary;
    translate: 3px;
    border: solid 1px $secondary;

    &.class-btn-unlike-userprofil>.img-att-userprofil:before {
      color: $secondary;
    }
  }
}

div.profilusermain-post {
  display: flex;
  max-width: 480px;
  width: 100%;
  max-height: 500px;
  flex-direction: column;
  margin-bottom: 2%;
  // padding-bottom: 1%;
  border: solid 2px;
  border-radius: 2%;
  justify-content: center;
  align-items: center;

  &:hover {
    // border-color:green;

    transform: scale(1.03);
    transition: ease 0.5s;
  }
}

p.card-profil-post-p {
  display: flex;
  flex-direction: row;
  margin: 0;
  padding-left: 1%;
  // padding-bottom: 2%;
  // border: 2px solid $primary;
  cursor: default;
}

</style>
  
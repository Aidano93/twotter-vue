<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{user.username}}</h1>
      <div class="user-profile__admin-badge" v-if="user.isAdmin" >Admin</div>
      <div class="user-profile__admin-badge" v-else>Not Admin</div>
      <div class="user-profile__follower-count">
        <strong>Followers:</strong> {{followers}}
      </div>
      <form class="user-profile__create-twoot" @submit.prevent="createNewTwoot">
        <label for="newTwoot"><strong>New Twoot</strong></label>
        <textarea name="newTwoot" id="newTwoot" rows="4" v-model="newTwootContent"></textarea>
        <div class="user-profile__create-twoot-type">
          <label for="newTwootType"><strong>Type</strong></label>
          <select id="newTwootType" v-model="selectedTwootType">
            <option :value="option.value" v-for="(option, index) in twootTypes" :key="index">
              {{option.name}}
            </option>
          </select>
        </div>
        <button>
          Post!
        </button>
      </form>
    </div>
    <div class="user-profile__twoots-wrapper">
      <TwootItem 
        v-for="twoot in user.twoots" 
        :key="twoot.id" 
        :username="user.username" 
        :twoot="twoot" 
        @favourite="toggleFavourite" 
      /> 
    </div>
  </div>
</template>
<script>
import TwootItem from "./TwootItem"

export default {
  
  name: 'UserProfile',
  components: {
    TwootItem,
  },
  data() {
    return {
      twootDate: '',
      newTwootContent: '',
      selectedTwootType: 'instant',
      twootTypes: [
        {value: 'draft', name: 'Draft'},
        {value: 'instant', name: 'Instant Twoot'}
      ],
      followers: 0,
      user: {
        id: 1,
        username: 'bandaids20',
        firstName: 'Aidan',
        lastName: "O'Reilly",
        email: 'ajoreilly93@gmail.com',
        isAdmin: true,
        twoots: [
          {id: 1, content: 'Twotter is cool.', date: '12/29/2021'},
          {id: 2, content: 'Yadda Yadda Yadda', date: '11/14/2021'},
          {id: 3, content: 'APES to the moon', date: '8/8/2021'}
        ]
      }
    }
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower!`)
      }
    }
  },
  
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`
    }
  },
  methods: {
    followUser() {
      this.followers++
    },
    toggleFavourite(id) {
      console.log(`Favourited Twoot ${id}`)
    },
    createNewTwoot() {
      if (this.newTwootContent && this.selectedTwootType !== 'draft') {
        this.user.twoots.unshift({
          id: this.user.twoots.length + 1,
          content: this.newTwootContent,
          date: this.twootDate,
        });
        this.newTwootContent = '';
      }
    },
  },
  mounted() {
    this.followUser();
    const today = new Date();
    const dd = String(today.getDate()).padStart(2, '0');
    const mm = String(today.getMonth() + 1).padStart(2, '0');
    const yyyy = today.getFullYear();

    this.twootDate = `${mm}/${dd}/${yyyy}`;
  },
}
</script>

<style lang="scss" scoped>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  width: 100%;
  padding: 50px 5%;

  .user-profile__user-panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #DFE3E8;

    h1 {
      margin: 0;
    }
    .user-profile__admin-badge {
      background: goldenrod;
      color: white;
      border-radius: 5px;
      margin-right: auto;
      padding: 0 10px;
      font-weight: bold;
    }
    .user-profile__create-twoot {
      padding-top: 20px;
      display: flex;
      flex-direction: column;
    }
  }
  .user-profile__twoots-wrapper {
      display: grid;
      grid-gap: .8rem;
    }
}




</style>
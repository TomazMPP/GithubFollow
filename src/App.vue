<template>
  <div id="main">
    <UserDetails @submit="showFollowingUsers" v-if="showUserDetails" />
    
    <div v-else>
      <div class="seguidores">
      <h2>PEOPLE THAT DON'T FOLLOW YOU BACK:</h2>
      <div class="usuarios">
        <div v-for="user in NonFollow" :key="user">
  <a :href="'https://github.com/' + user.login" target="_blank">
    
    <p><img :src="user.avatar_url" alt="avatar" class="avatar"> {{ user.login }}</p>
  </a>
    </div>
  </div>
      <button class="button" @click="goback">
   <svg class="svgIcon" viewBox="0 0 512 512" height="1em" xmlns="http://www.w3.org/2000/svg"><path d="M256 512A256 256 0 1 0 256 0a256 256 0 1 0 0 512zm50.7-186.9L162.4 380.6c-19.4 7.5-38.5-11.6-31-31l55.5-144.3c3.3-8.5 9.9-15.1 18.4-18.4l144.3-55.5c19.4-7.5 38.5 11.6 31 31L325.1 306.7c-3.2 8.5-9.9 15.1-18.4 18.4zM288 256a32 32 0 1 0 -64 0 32 32 0 1 0 64 0z"></path></svg>
  Try another user
</button>
    </div>
    </div>
    </div>
</template>

<script>
import UserDetails from './components/UserDetails.vue';

export default {
  name: 'App',
  components: {
    UserDetails
  },
  data() {
    return {
    showUserDetails: true,
    submittedUserName: '',
    userLogins: [],    // Add this line
    userFollowing: [], // Add this line
    NonFollow: [],     // Add this line
  };
  },
  methods: {
  async showFollowingUsers(username) {
    this.showUserDetails = false;
    this.submittedUserName = username;
    this.userLogins = [];
    this.userFollowing = [];
    this.NonFollow = [];

    try {
      // Obter seguidores
      const followersResponse = await fetch(`https://api.github.com/users/${username}/followers`);
      if (!followersResponse.ok) {
        throw new Error(`GitHub API request failed with status ${followersResponse.status}`);
      }

      const followersData = await followersResponse.json();

      followersData.forEach(follower => {
        this.userLogins.push(follower.login);
      });
    } catch (error) {
      console.error(error);
    }

    try {
      // Obter usuários seguidos
      const followingResponse = await fetch(`https://api.github.com/users/${username}/following`);
      if (!followingResponse.ok) {
        throw new Error(`GitHub API request failed with status ${followingResponse.status}`);
      }

      const followingData = await followingResponse.json();

      // Obter o avatar_url de cada usuário seguido
      for (const following of followingData) {
        try {
          const userResponse = await fetch(`https://api.github.com/users/${following.login}`);
          if (userResponse.ok) {
            const userData = await userResponse.json();
            this.NonFollow.push({
              login: userData.login,
              avatar_url: userData.avatar_url
            });
          }
        } catch (error) {
          console.error(error);
        }
      }
    } catch (error) {
      console.error(error);
    }

    this.$emit('userLogins', this.NonFollow);

    },
    goback() {
      this.showUserDetails = true;
    },
    
  }
};
</script>




<style scoped>
h2 {
    font-size: 30px;
    font-weight: 800;
    color: #32296b;
}

.seguidores {
display: flex;
flex-direction: column;
align-items: center;
}

.usuarios {
  padding: 5%;
    background-color: rgb(155, 81, 81);
    border-radius: 10px;
    margin: 2%;
    width: 80%;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
}

a {
  color: inherit;
  text-decoration: none;
}

.usuarios p {
  display: flex;
  align-items: center;
  padding: 10px;
  margin: 10px;
  text-align: center;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f0f0f0;
  font-weight: 600;
  font-size: 10px;
}

.avatar {
  width: 20px; /* Adjust the size as needed */
  height: 20px;
  border-radius: 50%;
  margin-right: 5px; /* Add margin to create space between avatar and username */
}

.usuarios p:hover {
  background-color: #5e4dcd;
}
.button {
  width: 180px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  background-color: #5e4dcd;
  border-radius: 30px;
  color: rgb(255, 255, 255);
  font-weight: 600;
  border: none;
  position: relative;
  cursor: pointer;
  transition-duration: .2s;
  box-shadow: 5px 5px 10px rgba(0, 0, 0, 0.116);
  padding-left: 8px;
  transition-duration: .5s;
  font-family: Poppins, sans-serif;
}

.svgIcon {
  height: 25px;
  transition-duration: 1.5s;
}

.bell path {
  fill: rgb(19, 19, 19);
}

.button:hover {
  background-color: #9589e2;
  transition-duration: .5s;
}

.button:active {
  transform: scale(0.97);
  transition-duration: .2s;
}

.button:hover .svgIcon {
  transform: rotate(250deg);
  transition-duration: 1.5s;
}

@media only screen and (max-width: 600px) {
  .usuarios {
    width: 100%;
    grid-template-columns: repeat(2, 1fr);
  }

  h2 {
    font-size: 18px;
  }
}

@media only screen and (min-width: 601px) and (max-width: 1024px) {
  .usuarios {
    width: 100%;
    grid-template-columns: repeat(2, 1fr);
  }

  h2 {
    font-size: 26px;
  }
}

</style>

<template>
  <section>
    <h2>{{ teamName }}</h2>
    <ul>
      <user-item
        v-for="member in members"
        :key="member.id"
        :name="member.fullName"
        :role="member.role"
      ></user-item>
      <router-link to="/teams/t2">Go to team2</router-link>
    </ul>
  </section>
</template>

<script>
import UserItem from '../users/UserItem.vue';

export default {
  inject: ['users', 'teams'],
  props:['teamId'],
  components: {
    UserItem,
  },
  data() {
    return {
      teamName: '',
      members: [],
    };
  },

  methods: {
    loadTeamMember(teamId) {
      const selectedTeam = this.teams.find((team) => teamId === team.id);
      //the code above is to ensure that the params is same with the data provided
      const members = selectedTeam.members;
      const selectedMembers = [];
      for (const member of members) {
        const selectedUser = this.users.find((user) => user.id === member);
        selectedMembers.push(selectedUser);
      }

      this.members = selectedMembers;
      this.teamName = selectedTeam.name;
    },
  },

  beforeRouteUpdate(to, from, next){
    // this.loadTeamMember(to.params.teamId)
    next()
  },  

  created() {
    this.loadTeamMember(this.teamId)
  },

  watch: {
    teamId(newId) {
      this.loadTeamMember(newId)
    },
  },
};
</script>

<style scoped>
section {
  margin: 2rem auto;
  max-width: 40rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 1rem;
  border-radius: 12px;
}

h2 {
  margin: 0.5rem 0;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
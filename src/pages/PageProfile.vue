<template>
  <div class="flex-grid">
    <UserProfileCard
      v-if="!edit"
      :user="user"
    />
    <UserProfileCardEditor
      v-else
      :user="user"
    />

    <div class="col-7 push-top">
      <div class="profile-header">
        <span class="text-lead">
            {{user.username}}'s recent activity
        </span>
        <a href="#">See only started threads?</a>
      </div>

      <hr>
      <PostList :posts="userPosts"/>
    </div>
  </div>
</template>

<script>
    import PostList from '@/components/PostList' //importimi i komponentes per listen e posteve
    import UserProfileCard from '@/components/UserProfileCard' // importimi i komponentes per te dhenat e userit
    import UserProfileCardEditor from '@/components/UserProfileCardEditor' //importimi i komponentes per editimin e te dhenave
    import {mapGetters} from 'vuex' // mapGetters na mundeson te marim userin e autentifikuar
    import asyncDataStatus from '@/mixins/asyncDataStatus' //collect te dhenave nga mixin

    export default {
      components: {
        PostList,
        UserProfileCard,
        UserProfileCardEditor
      },

      mixins: [asyncDataStatus],

      props: {
        edit: {
          type: Boolean,
          default: false
        }
      },

      computed: {
        //Fetch userin qe eshte i autentifikuar
        ...mapGetters({
          user: 'authUser'
        }),
      //fetch postet e userit te autentifikuar permes getters per t'i renderuar ne komponenten PostList
        userPosts () {
          return this.$store.getters.userPosts(this.user['.key'])
        }
      },
      created () {
        this.$store.dispatch('fetchPosts', {ids: this.user.posts})
          .then(() => this.asyncDataStatus_fetched())
      }
    }
</script>

<style scoped>

</style>
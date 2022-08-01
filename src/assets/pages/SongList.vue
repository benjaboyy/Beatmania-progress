<template>
  <div class="container-fluid bg-dark">
    <div class="row text-center">
      <div class="col-12 p-0 text pt-4">
        <h1 class="text-center">Beatmania</h1>
        <h2 class="mb-3">The Progress</h2>
        <div class="btn-group w-100" role="group" aria-label="Basic example">
          <button type="button" class="btn btn-primary btn-lg" data-bs-toggle="modal" data-bs-target="#ModalDifficulty">MIX</button>
          <button type="button" class="btn btn-secondary btn-lg" data-bs-toggle="modal" data-bs-target="#ModalLevel">Difficulty</button>
          <button @click="loadData"  type="button" class="btn btn-primary btn-lg">Reload data</button>
        </div>
        <table v-if="isMounted" class="table table-borderless bg-light table-striped">
          <tbody>
            <tr v-for="song in songs" :key="song.songID">
              <td class="d-none d-md-block"
                  v-for="mix in calcMix(song)"
                  :key="mix.mixID">{{ mix.name }}
              </td>
              <th class="text-start">{{ song.name }}</th>
              <td class="d-none d-md-block">{{ song.artist }}</td>
              <td>{{ song.bpm }}</td>
              <td class="text-white diff-td"
                  v-for="diff in calcDiff(song)"
                  @click="toggleDiff(diff.difficultyID)"
                  :key="diff.difficultyID"
                  :class="(diff.difficulty !== null ? 'bg-theme_' + diff.groupID:'bg-dark') + ' ' + (isDifficultyPassed(diff.difficultyID) ? 'passed' : '')">
                  {{ diff.difficulty }}
              </td>
              <td><i class="fa fa-star text-primary"></i> </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
  <modal-difficulty></modal-difficulty>
  <modal-level></modal-level>
</template>



<script>
import ModalDifficulty from "@/assets/components/ModalDifficulty";
import ModalLevel from "@/assets/components/ModalLevel";

export default {
  data() {
    return{
      accountID: 0,
      isMounted: false,
      songs: [],
      mixes: [],
      difficulties: [],
      userDifficulty: []
    }
  },
  mounted() {
    this.loadData()
    this.isMounted = true
  },
  methods: {
    loadData() {
      fetch('http://localhost:3000/songs')
          .then(res => res.json())
          .then(data => this.songs = data)
          .catch(err => console.log(err.message))

      fetch('http://localhost:3000/difficulties')
          .then(res => res.json())
          .then(data => this.difficulties = data)
          .catch(err => console.log(err.message))

      fetch('http://localhost:3000/mixes')
          .then(res => res.json())
          .then(data => this.mixes = data)
          .catch(err => console.log(err.message))

      fetch('http://localhost:3000/userDifficulty')
          .then(res => res.json())
          .then(data => this.userDifficulty = data)
          .catch(err => console.log(err.message))
    },
    calcDiff(song) {
      console.log(song)
      return this.difficulties.filter(difficulty => {
        return difficulty.songID === song.songID
      })
    },
    calcMix(song) {
      console.log(song)
      return this.mixes.filter(mix => {
        return mix.mixID === song.mixID
      })
    },
    isDifficultyPassed(id) {


      const me = this
      const has = this.userDifficulty.find(function(difficultyRow) {

        if(difficultyRow.difficultyID != id) {
          return false
        }
        if(difficultyRow.userID != me.accountID) {
          return false
        }
        return true

      })

      return has != undefined

    },
    toggleDiff(id) {
      const me = this

      if(!this.isDifficultyPassed(id)) {

        this.userDifficulty.push({
          userID: this.accountID,
          difficultyID: id
        });

      } else {

        this.userDifficulty = this.userDifficulty.filter(function(difficultyRow) {

          if(difficultyRow.difficultyID == id && difficultyRow.userID == me.accountID) {
            // Don't keep (remove)
            return false
          } else {
            // Keep
            return true
          }

        })

      }
    }
  },
  name: "SongList",
  components: {
    ModalDifficulty,
    ModalLevel
  }
}

</script>

<style scoped>
.passed {
  color: grey!important;

}
</style>
<script setup>
import { ref, computed, unref, watch } from 'vue';
import { useRouter } from 'vue-router'
const router = useRouter()
import listOfFiles from '../listOfFiles.json'

// https://flexbox.help/
// https://nerdcave.com/tailwind-cheat-sheet

// const possibleLanguages=Object.keys(listOfFiles).sort()
// let language=ref(possibleLanguages[0])

// const possibleTypes=computed(()=>Object.keys(listOfFiles[language.value]))
// let typeIndex=ref(0)
// let type=computed(()=>possibleTypes.value.length>0?possibleTypes.value[typeIndex.value]:'')
// // watchEffect(() => type=possibleTypes.value[0])
// watch(
//   language, (language, prev) => {
//     typeIndex.value=0
//   }
// )

// let possibleFiles=computed(()=>{
// 	let liste=listOfFiles[unref(language)][unref(type)]
// 	let result = liste.sort((a, b) => a.name.localeCompare(b.name))
// 	return result
// })
// let fileIndex=ref(0)
// let file=computed(()=>possibleFiles.value.length>0?possibleFiles.value[fileIndex.value]:'')
// watch(
//   typeIndex, (typeIndex, prev) => {
//     fileIndex.value=0
//   }
// )
let languages = ['French','Spanish','Portuguese','Italian','Vietnamese','Turkish','Japanese','Chinese','Korean','Thai','Russian','Greek','Hebrew','Arabic','Persian','Hindi'].sort()
let language=ref(languages[0])

let categories=['to english', 'from english']
let category=ref(categories[0])

let allChannels={"Korean":['allo']}
let channels=computed(()=>{
	let result = []
	if(category.value==='youtube')
		result = allChannels[language.value] ? allChannels[language.value] : []
	else
		result = [] 
	if(result.length === 0)
		channel.value=''
	else
		channel.value=result[0]
	return result
})
let channel = ref('')

let fileIndex=ref(0)
watch(language, (language, previous) => {
  fileIndex.value=0
})
watch(category, (category, previous) => {
  fileIndex.value=0
})
watch(channel, (category, previous) => {
  fileIndex.value=0
})
let filteredListOfFiles=computed( () => listOfFiles.filter(a => ( !a.language.indexOf(language.value) && !a.category.indexOf(category.value) && !a.channel.indexOf(channel.value) ) ) )
let file=computed(()=>filteredListOfFiles.value[fileIndex.value])
let email=ref('')

let isDisabled=computed(()=>
	!(email.value!='' && unref(file).name!='')
)

let buttonClass=computed(()=>{
	if(!isDisabled.value)
		return 'my-button'
	else
		return 'my-button-disabled'
})

const move = () => {
  if (!isDisabled.value)
    router.push({ 
			name:'Suite',
      params: {
        fileName: unref(file).name, // encodeURIComponent
				price: unref(file).price,
				email: unref(email),
				language: unref(language),
				docType: unref(category),
				channel:unref(file).channel,
				id: unref(file).id
      }
    })
	else{
		window.alert('Please write your email adress so that we can send you the document.')
	}
}
</script>


<template>

  <div class="liste">
		<img src="../assets/languages2.jpeg" alt="" width="120" height="120">
		<h1 class="text-3xl mb-3 mt-2">
			NaturaLingua
		</h1>
		<div class="text-base">
			With our documents, you will be able to learn languages in a natural way.<br>Please pick the language, the type of document and finally the document you are interested in.<br><a href="https://www.dropbox.com/sh/xjer747bcmkukwp/AADGKn4rRadupTr-CYapl8oya?dl=0
" target="_blank" rel="noopener noreferrer">Samples from the pdf documents</a>
		</div> 
		
		<div>
			language: 
			<select v-model="language" class="m-2 rounded border-1 border-black">
				<option v-for="bidule in languages" :id="bidule">{{bidule}}</option>
			</select>
		</div>

		<div>
			category :
			<select v-model="category" class="m-2 rounded border-1 border-black">
      	<option v-for="bidule in categories" :id="bidule">{{bidule}}</option>
    	</select>
		</div>

    <!-- <select v-model="typeIndex" class="m-2 rounded border-1 border-black">
      <option v-for="(bidule2, index) in possibleTypes" :id="index" :value="index">{{bidule2}}</option>
    </select> -->

		<div v-if="channels.length!=0">
			youtube channel :
			<select v-model="channel" class="m-2 rounded border-1 border-black">
				<option v-for="bidule in channels" :id="bidule">{{bidule}}</option>
			</select>
		</div>

		<div v-if="filteredListOfFiles.length!= 0">
			document :
			<select  v-model="fileIndex" class="m-2 rounded border-1 border-black">
				<template v-for="(bidule3, index) in filteredListOfFiles">
					<option :id="index" :value="index">{{bidule3.name}} ({{bidule3.price}} euros)</option>
				</template>
			</select>
		</div>
		<div v-else>
			Sorry, there is no document for this language and category.
		</div>

		<div>
			email :
			<input autocomplete="email" type="text" class="m-2 rounded border-1 border-black m-2" v-model="email" placeholder="type your email here"/>
		</div>

    <button :class="buttonClass" @click="move"> 
		<!-- my-button-disabled -->
      Go to purchase summary
    </button>
		<br><br><br>
		<p>Other websites by NaturaLingua:
      <ul>
        <li>
          <a href="https://getyoutubesubtitles.netlify.app">https://getyoutubesubtitles.netlify.app</a>
        </li>
        <li>
          <a href="https://getmoviessubtitles.netlify.app">https://getmoviessubtitles.netlify.app</a>
        </li>
      </ul>
    </p>
		<br><br><br>
		<a href="https://www.flaticon.com/free-icons/languages" title="languages icons">Languages icons created by Freepik - Flaticon</a>
  </div>

</template>

<style scoped>
a {
  color: blue;
  text-decoration: underline blue;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.liste{
	padding: 0.8em;
	display: flex;
	flex-direction: column;
	flex-wrap: nowrap;
	justify-content: flex-start;
	align-items: center;
	align-content: space-between;
}
.my-button{
  @apply bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mt-4
}
.my-button-disabled{
	/* pointer-events:none; */
	cursor : default !important;
  @apply bg-gray-500 text-white font-bold py-2 px-4 rounded mt-4
}
</style>

<template>
	<form class="m-4" v-on:submit.prevent="addNote()">
		<div class="mb-3">
			<label for="exampleFormControlInput1" class="form-label">Tytuł notatki</label>
			<input type="text" class="form-control" id="exampleFormControlInput1" v-model="newNoteName">
			</div>
			<div class="mb-3">
				<label for="exampleFormControlTextarea1" class="form-label">Treść</label>
				<textarea class="form-control" id="exampleFormControlTextarea1" rows="3" v-model="newNoteContent"></textarea>
			</div>
			<button type="submit" class="btn btn-primary w-100">
				<i class="bi bi-save"></i>
				Dodaj notatkę
			</button>
		</form>
	</template>
	
	<script>
		export default {
		components: {},
		data: function (){
		return {
		newNoteName: null,
		newNoteContent: null,
		}
		},
		methods: {
		addNote: function (){
		var date = (new Date()).toLocaleString("pl-Pl");
		var note = { name: this.newNoteName, content: this.newNoteContent, createdAt: date};
		this.$emit("on-add-note", note);
		this.newNoteName = null;
		this.newNoteContent = null;
		}
		},
		}
	</script>
	
	..........................
	
	App.vue:
	
	<template>
		<div class="container-fluid p-0 m-0 vh-100">
			<div class="row p-0 m-0 h-100">
				<div class="col-12 col-md-5 col-lg-4 bg-light p-0 h-100">
					<h3 class="w-100 text-center p-4">
						<i class="bi bi-clipboard-check text-secondary"></i>
						GoNote
					</h3>
					<button type="button" class="btn btn-dark w-100 py-2 rounded-0" v-on:click="showNotAddForm()">
						<i class="bi bi-pen-fill"></i>
						Stwórz nową notatkę
					</button>
					<div v-if="notes.length == 0" class="alert alert-secondary m-3 text-center">
						<i class="bi bi-emoji-frown"></i>
						Jeszcze nie ma żadnych notatek do wyświetlenia
					</div>
					<notes-list v-else v-bind:notes="notes" v-on:onShowNote="showNote($event)" v-on:onDeleteNote="deleteNote($event)"></notes-list>
				</div>
				<div class="col-12 col-md-7 col-lg-8 bg-white p-0">
					<div class="cover d-none d-sm-block"></div>
					<note v-if="currentNote !== null" v-bind:note="currentNote"></note>
					<note-add-form v-else v-on:onAddNote="addNote($event)"></note-add-form>
				</div>
			</div>
		</div>
		<nav class="navbar fixed-bottom navbar-dark bg-dark">
			<div class="container-fluid justify-content-end p-2">
				<span class="text-muted mx-5 d-none d-md-block">
					©2021 Polityka prywatności | Warunki korzystania z usługi
				</span>
				<span class="text-light">
					<i class="bi bi-heart-fill"></i>
					Moje konto
				</span>
			</div>
		</nav>
	</template>
	
	<script>
		import NoteAddForm from "./components/NoteAddForm.vue";
		import NotesList from "./components/NotesList.vue";
		import Note from "./components/Note.vue";
		
		export default {
		components: {NoteAddForm, NotesList, Note},
		data: function (){
		return {
		notes: [],
		currentNote: null,
		}
		},
		methods: {
		addNote: function (note){
		this.notes.push(note);
		},
		showNote: function (note){
		this.currentNote = note;
		},
		showNotAddForm: function (){
		this.currentNote = null;
		},
		deleteNote: function (note){
		this.notes = this.notes.filter((n) => n !== note);
		if (this.currentNote == note){
		this.currentNote = null;
		}
		},
		},
		}
	</script>
	
	<style>
		.cover {
		background-image: url("./assets/background.jpg");
		background-position: center;
		background-size: cover;
		min-height: 150px;
		}
	</style>		
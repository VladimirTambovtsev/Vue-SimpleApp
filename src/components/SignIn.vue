<template>
	<div>
		<form class="mt-5" @submit.prevent="enterUser" v-if="show">
			<div class="form-group">
				<label for="email">Email</label>
				<input type="email" class="form-control" id="email" placeholder="Введите Email" 
				v-model="user.email"  required />
			</div>
			<div class="form-group">
				<label for="password">Пароль</label>
				<input type="password" class="form-control" id="password" placeholder="Введите Пароль"
				v-model="user.password" required />
			</div>
			<button type="submit" class="btn btn-primary">Войти</button>
		</form>
		<div class="alert alert-success mt-5" role="alert" v-if="signSuccess">
			<strong>Отлично!</strong>Вы вошли в систему
		</div>
		<div class="alert alert-danger mt-5" role="alert" v-if="signError">
			<strong>Упс!</strong>Что-то пошло не так.
		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				show: true,
				signSuccess: false,
				signError: false,
				user: {
					email: '',
					password: ''
				}
			}
		},
		methods: {
			enterUser() {
				firebase.auth().signInWithEmailAndPassword(this.user.email, this.user.password)
					.then(res => {
						const settings = {
							email: res.email,
							signComplete: true,
							mainPage: true,
							uid: res.uid
						}
						this.$emit('addUser', settings);
						this.show =false;
						this.signSuccess = true;
					})
					.catch(err => {
						this.signError = true;
					});
			}
		}
	}
</script>


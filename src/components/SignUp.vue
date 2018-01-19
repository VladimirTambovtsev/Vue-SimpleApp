<template>
	<div>
		<form class="mt-5" @submit.prevent="registerUser" v-if="show">
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
			<div class="form-group">
				<label for="password">Повторите пароль</label>
				<input type="password" class="form-control" id="password2" placeholder="Введите Пароль"
				v-model="user.confirmPassword" required />
			</div>
			<div class="alert alert-danger" role="alert" v-if="errorConfirm">
				<strong>Упс..</strong> Пароли не совпадают
			</div>
			<div class="alert alert-danger" role="alert" v-if="errorSmall">
				<strong>Упс..</strong> Пароли не совпадают
			</div>
			<button type="submit" class="btn btn-primary">Зарегистрироваться</button>
		</form>
		<div class="alert alert-success" role="alert" v-if="signSuccess">
			<strong>Отлично</strong> Регистрация прошла успешно
		</div>
		<div class="alert alert-danger" role="alert" v-if="signError">
			<strong>Упс</strong> Что-то пощло не так
		</div>
	</div>
</template>

<script>
	export default {
		name: 'sign-up',
		data() {
			return {
				show: true,
				signSuccess: false,
				signError: false,
				user: {
					email: '',
					password: '',
					confirmPassword: ''
				},
				errorConfirm: false,
				errorSmall: false
			}
		},
		methods: {
			registerUser() {
				this.errorConfirm = false;
				this.errorSmall = false;
				if (this.user.password !== this.user.confirmPassword) {
					this.errorConfirm = true;
				} else if (this.user.password.length < 6){
					this.errorSmall = true;
				} else {
					firebase.auth().createUserWithEmailAndPassword(this.user.email, this.user.password)
						.then(() => {
							this.$emit('regSuccess', 'sign-in');	// if success, add Listener
							this.show = false;
							this.signSuccess = true;
						})
						.catch(err => {
							this.signError = true;
						})
				}
			}
		}
	}
</script>
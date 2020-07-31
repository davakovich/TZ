<template>
	<div class="Global-div">
		<div class="Main-form">
			<v-form
					name="input_form"
					ref="form"
					v-model="input_form"
					id="Formsotr"
					@submit.prevent="Onsubmit"
			>
				<h2>{{main_header}}</h2>
				<p>Сотрудник</p>
				<v-textarea
						type="text"
						v-model="fio_s"
				/>
				<p>Серия паспорта</p>
				<v-text-field
						type="number"
						v-model="pass_ser"
						max="9999"
				/>
				<p>Номер паспорта</p>
				<v-text-field
						type="number"
						v-model="pass_no"
						max="999999"
				/>
				<p>Дата выдачи</p>
				<v-text-field
						type="date"
						v-model="pass_dt"
				/>
				<p></p>
				<span>
					<v-btn
							v-model="submit_btn"
							type="submit"
							class="good-button"
					>Сохранить</v-btn>
					<v-btn
							v-model="delete_btn"
							type="button"
							class="bad-button"
							v-on:click="delSotr"
							id="delete"
							value="Удалить"
					>Удалить</v-btn>
				</span>
			</v-form>
		</div>
		<div
				class="Sotrlist"
				@submit.prevent="Onsubmit"
		>
			<ul>
				<v-btn
						class="button"
						v-on:click="addsotr"
				>+</v-btn>
				<p></p>
				<Informat
						v-for="sotr of sotr"
						:sotr="sotr"
						:key="sotr[0]"
						v-on:filler="fillersotr"
						v-on:addsotr="addsotr"
						v-on:delSotr="delSotr"
				/>
			</ul>
		</div>
	</div>
</template>
<script>
	import Informat from "./Informat";
	import App from "../App";
	export default {
		data() {
			return {
				input_form: "",
				fio_s: "",
				pass_ser: "",
				pass_no: "",
				pass_dt: "",
				main_header: "",
				submit_btn: "",
				delete_btn: ""
			}
		},
		props: ["sotr"],
		components: {
			Informat
		},
		methods: {
			delSotr() {
				alert(App.methods.localGet("Massotr"))
				if (App.methods.localGet("del") != null) {
					let b = localStorage.getItem("del")
					let sotr = App.methods.localGet("Massotr")
					sotr.splice(b, 1)
					localStorage.setItem("Massotr", JSON.stringify(sotr))
					localStorage.removeItem("del")
					this.input_form.hidden = true
					alert("Данные сотрудника удалены")
				} else {
					this.input_form.hidden = true
				}
			},
			addsotr() {
				localStorage.removeItem("del")
				this.input_form.clear()
				this.main_header = "Добавить сотрудника"
				document.getElementById("delete").value = "Сброс"
				document.getElementById("Formsotr").hidden = false
			},
			Onsubmit() {
				if (JSON.parse(localStorage.getItem("del")) == null) {
					let val = [
						this.fio_s,
						this.pass_ser,
						this.pass_no,
						this.pass_dt
					]
					let a = JSON.parse(localStorage.getItem("Massotr"))
					a.push(val);
					localStorage.setItem("Massotr", JSON.stringify(a))
					document.getElementById("Formsotr").hidden = true;
					alert("Сотрудник успешно добавлен")
				} else {
					let a = JSON.parse(localStorage.getItem("Masssotr"))
					let b = localStorage.getItem("del")
					a[b][0] = this.fio_s
					a[b][1] = this.pass_ser
					a[b][2] = this.pass_no
					a[b][3] = this.pass_dt
					localStorage.setItem('Masssotr', JSON.stringify(a))
					localStorage.removeItem("del")
					alert("Данные сотрудника успешно обновлены")
				}
			},
			fillersotr(fio) {
				this.delete_btn = "Удалить"
				this.main_header = "Редактировать сотрудника"
				let b = 0
				let sotr = JSON.parse(localStorage.getItem("Massotr"))
				for (let i = 0; i < sotr.length; i++)
					if (sotr[i][0] == fio)
						b = i
				this.fio_s = sotr[b][0]
				this.pass_ser = sotr[b][1]
				this.pass_no = sotr[b][2]
				this.pass_dt = sotr[b][3]
				localStorage.setItem("del", b)
			}
		}
	}

</script>
}
}
<style scoped>
	li {
		border: 1px solid #ccc;
		display: flex;
		justify-content: space-between;
		padding: .5rem 2rem;
		margin-bottom: 1rem;
	}

	.Global-div {
		width: 30%;
		float: left;
	}

	.Main-form {
		position: absolute;
		left: 30%;
		right: 40%;

	}
	.button {
		background-color: lightgray;
		border-radius: 50%;
		color: green;
		text-align: center;
		text-decoration: none;
		font-size: 20px;
		cursor: pointer;
	}
	.bad-button {
		background-color: red;
		color: white;
		text-align: center;
		text-decoration: none;
		font-size: 15px;
		cursor: pointer;
	}
	.good-button {
		background-color: green;
		color: white;
		text-align: center;
		text-decoration: none;
		font-size: 15px;
		cursor: pointer;
	}
</style>

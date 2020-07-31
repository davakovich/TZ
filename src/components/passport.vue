<template>
	<div class="Global-div">
		<div class="Main-form">
			<v-form
					name="input_form"
					ref="form"
					v-model="input_form"
					@submit.prevent="onSubmit"
			>
				<h2>{{main_header}}</h2>
				<p>Сотрудник</p>
				<v-text-field
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
							v-on:click="delStaff"
							id="delete"
					>{{delete_btn}}</v-btn>
				</span>
			</v-form>
		</div>
		<div
				class="Stafflist"
				@submit.prevent="Onsubmit"
		>
			<ul>
				<v-btn
						class="button"
						v-on:click="addStaff"
				>+
				</v-btn>
				<p></p>
				<Informat
						v-for="emp_mas of emp_mas"
						:emp_mas="emp_mas"
						:key="emp_mas[0]"
						v-on:filler="fillerStaff"
						v-on:addStaff="addStaff"
						v-on:delStaff="delStaff"
				/>
			</ul>
		</div>
	</div>
</template>
<script>
	import Informat from "./Informat";
	import { localGet } from "../App";
	import { localSet } from "../App";
	import { localRemove } from "../App";
	export default {
		data() {
			return {
				input_form: "",
				fio_s: "",
				pass_ser: "",
				pass_no: "",
				pass_dt: "",
				main_header: "Выберите сотрудника",
				submit_btn: "",
				delete_btn: "Удалить",
				emp_mas: localGet("staff")
			}
		},
		components: {
			Informat
		},
		methods: {
			delStaff() {
				if(localGet("del") != null) {
					let b = localGet("del")
					let staff = localGet("staff")
					staff.splice(b, 1)
					localSet("staff", (staff))
					localRemove("del")
					this.emp_mas = staff
					this.$refs.form.reset()
					alert("Данные сотрудника удалены")
				}
				else{
					this.$refs.form.reset()
				}
			},
			addStaff() {
				localRemove("del")
				this.$refs.form.reset()
				this.main_header = "Добавить сотрудника"
				this.delete_btn = "Сброс"
				this.emp_mas = localGet("staff")
			},
			onSubmit() {
				if (localGet("del") == null) {
					let val = [
						this.fio_s,
						this.pass_ser,
						this.pass_no,
						this.pass_dt
					]
					let a = localGet("staff")
					a.push(val);
					localSet("staff", a)
					alert("Сотрудник успешно добавлен")
				} else {
					let a = localGet("staff")
					let b = localGet("del")
					a[b][0] = this.fio_s
					a[b][1] = this.pass_ser
					a[b][2] = this.pass_no
					a[b][3] = this.pass_dt
					localSet("staff",a)
					localRemove("del")
					alert("Данные сотрудника успешно обновлены")
				}
				this.emp_mas = localGet("staff")
			},
			fillerStaff(fio) {
				this.delete_btn = "Удалить"
				this.main_header = "Редактировать сотрудника"
				let b = 0
				let staff = localGet("staff")
				for (let i = 0; i < staff.length; i++)
					if (staff[i][0] == fio)
						b = i
				this.fio_s = staff[b][0]
				this.pass_ser = staff[b][1]
				this.pass_no = staff[b][2]
				this.pass_dt = staff[b][3]
				localSet("del", b)
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
		width: 20%;
		float: left;
	}

	.Main-form {
		width: 20%;
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

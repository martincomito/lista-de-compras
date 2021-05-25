<template>
  <Page>
    <ActionBar>
      <Label text="Home" />
    </ActionBar>

    <FlexboxLayout flexDirection="column">
			<Label class="info">
				Productos por comprar
			</Label>
			<ScrollView orientation="horizontal">
				<ListView height="250" for="item in list" class="pending-items">
					<v-template>
						<StackLayout orientation="horizontal" @tap="markAsDone(item)">
							<Label :key="item.id">
								<FormattedString>
									<Span class="item" :text="item.name" :class="{ 'disabled': !item.active }" />
								</FormattedString>
							</Label>
						</StackLayout>
					</v-template>
				</ListView>
			</ScrollView>
			<Label class="info">
				Productos comprados
			</Label>
			<ScrollView orientation="horizontal">
				<ListView height="250" for="item in done" class="finished-items">
					<v-template>
						<StackLayout orientation="horizontal" @tap="addBackToPending(item)">
							<Label :key="item.id">
								<FormattedString>
									<Span class="item" :text="item.name" :class="{ 'disabled': !item.active }" />
								</FormattedString>
							</Label>
						</StackLayout>
					</v-template>
				</ListView>
			</ScrollView>
			<ContentView class="add-task-button-container">
				<Button class="add-button" text="+" horizontalAlignment="right" borderWidth="2" @tap="openAddProductModal()" />
			</ContentView>	
    </FlexboxLayout>
  </Page>
</template>

<script>

export default {
  computed: {
		listIsEmpty() {
			return this.list.length === 0
		}
  },
	data() {
		return {
			list: [
				{ name: 'Lechuga', id: 1, active: true },
				{ name: 'Banana', id: 2, active: true },
				{ name: 'Papa', id: 3, active: true },
				{ name: 'Manzana', id: 4, active: true },
			],
			done: []
		}
	},
  methods: {
		markAsDone(item) {
			item.active = false
			this.done.push(item)
			this.list = this.list.filter(product => product.id !== item.id)
		},
		addProduct(productName) {
			this.list.push({ name: productName, id: this.list.length + 1, active: true })
		},
		addBackToPending(item) {
			item.active = true
			this.list.push(item)
			this.done = this.done.filter(product => product.id !== item.id)
		},
		openAddProductModal() {
			prompt({
				title: "Nuevo producto",
				message: "Ingrese el nombre:",
				okButtonText: "Guardar",
				cancelButtonText: "Cancelar"
			}).then(result => {
				this.addProduct(result.text)
			})
		}
	}
};
</script>

<style scoped lang="scss">
.label-empty-list {
	height: 250;
	font-size: 15;
	border-width: 2;
	border-color: gray;
}
.pending-items {
	flex: 4;
	border-width: 2;
	border-color: gray;
}
.finished-items {
	flex: 1;
	border-width: 2;
	border-color: gray;
}
.item {
	font-size: 15;
}
.info {
	align-self: center;
  font-size: 20;
  horizontal-align: center;
	margin-top: 15;
	margin-bottom: 15;
  // vertical-align: center;
}
.add-task-button-container {
	flex: 1;
}
.add-button {
	border-radius: 100%;
	font-weight: bold;
	font-size: 30;
	color: white;
	background-color: cornflowerblue;
	width: 80;
	height: 80;
	margin-right: 20;
	align-self: flex-end;
}
.disabled {
	color: gray;
	text-decoration: line-through;
}
</style>

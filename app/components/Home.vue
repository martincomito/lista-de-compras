<template>
  <Page>
    <ActionBar>
      <Label text="Home" />
    </ActionBar>

    <FlexboxLayout flexDirection="column">
			<Label class="info">
				Lista de compras
			</Label>
			<ListView for="item in list" class="home-panel">
				<v-template>
					<StackLayout orientation="horizontal" @tap="onItemTap(item)">
						<Label :key="item.id">
							<FormattedString>
								<Span class="item" :text="item.name" :class="{ 'disabled': !item.active }" />
							</FormattedString>
						</Label>
						<!-- <Switch :checked="item.active"/> -->
					</StackLayout>
				</v-template>
			</ListView>
			<ContentView class="home-panel-2">
				<Button class="add-button" text="+" horizontalAlignment="right" borderWidth="2" @tap="openAddProductModal()" />
			</ContentView>	
    </FlexboxLayout>
  </Page>
</template>

<script>

export default {
  computed: {
  },
	data() {
		return {
			list: [
				{ name: 'Lechuga', id: 1, active: true },
				{ name: 'Banana', id: 2, active: true },
				{ name: 'Papa', id: 3, active: false },
				{ name: 'Manzana', id: 4, active: false },
			],
		}
	},
  methods: {
		onItemTap(item) {
			item.active = !item.active
		},
		addProduct(productName) {
			this.list.push({ name: productName, id: this.list.length + 1, active: true })
			console.log(this.list)
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
.home-panel {
	flex: 4;
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
.home-panel-2 {
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
	text-decoration: line-through;
}
</style>

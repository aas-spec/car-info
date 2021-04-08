<template>
    <div>
        <div class="shadow w-75 ml-auto mr-auto rounded  view-container">
            <h5 class="text-center mb-3">Ввод информации об автомобиле</h5>
            <div class="form-row">
                <label class="control-label">VIN
                    <span class="badge  badge-secondary">{{selectedVIN.length}}</span>
                </label>
                <input
                        v-model="selectedVIN"
                        type="text"
                        class="form-control"
                        :class="{'border-danger': !isUntouchedVIN && !isValidVIN }"
                        @blur="untouchedVIN = false"
                />
            </div>
            <div class="form-row">
                <label class="control-label">Выберите марку</label>
                <fieldset class="w-100" :disabled="!isValidVIN">
                    <select v-model="selectedBrand" type="text" class="form-control">
                        <option value="" :key="undefined">Не выбрано</option>
                        <option v-for="brand in brands" :key="brand">
                            {{brand}}
                        </option>
                    </select>
                </fieldset>
            </div>
            <div class="form-row">
                <label class="control-label">Модель</label>
                <fieldset class="w-100" :disabled="!isValidBrand">
                    <input v-model="selectedModel" type="text" class="form-control"/>
                </fieldset>
            </div>
        </div>
        <div class="shadow w-75 ml-auto mr-auto rounded view-container">
            <h5 class="text-center mb-3">Список запчастей</h5>
            <div class="form-row" v-if="parts.length < 1">
                <label class="text-center m-2">Добавить</label>
                <button class="btn btn-primary" @click="addPart">+</button>
            </div>
            <div v-else class="form-row">
                <template v-for="part in parts">
               <span class="d-flex mt-1" :key="part.id">
                   <input class="form-control mr-2" v-model="part.name"/>
                   <button class="btn btn-primary ml-1" @click="addPart">+</button>
                   <button class="btn btn-danger ml-1" @click="deletePart(part.id)">-</button>
               </span>
                </template>
            </div>
        </div>
        <div class="shadow w-75 ml-auto mr-auto rounded view-container text-left">
            <h5 class="text-center mb-3">Информация</h5>
            <table class="table w-100">
                <tbody>
                    <tr>
                        <td>VIN</td>
                        <td>{{selectedVIN}}</td>
                    </tr>
                    <tr>
                        <td>Марка</td>
                        <td>{{selectedBrand}}</td>
                    </tr>
                    <tr>
                        <td>Модель</td>
                        <td>{{selectedModel}}</td>
                    </tr>
                    <tr>
                        <td>Список запчастей</td>
                        <td>{{partList}}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
    export default {
        name: "CarInfo",
        data() {
            return {
                brands: [
                    'Audi',
                    'Skoda',
                    'Toyota'
                ],
                selectedVIN: '',
                selectedBrand: '',
                selectedModel: '',
                parts: [],
                untouchedVIN: true,
            }
        },
        methods: {
            addPart() {
                // Получаю максимальный id  и устанавливаю
                const maxId = Math.max(0, ...this.parts.map(part => part.id));
                this.parts.push({id: maxId + 1, name: ''});
            },
            deletePart(id) {
                // Беру индекс в массиве, если нашел, удаляю
                const index = this.parts.findIndex(part => part.id === id);
                if (index >= 0) this.parts.splice(index, 1);
            }
        },
        computed: {
            isValidVIN() {
                const re = /^[\dABCDEFGHJKLMNPRSTUVWXYZ]{17}$/;
                return this.selectedVIN.match(re);
            },
            isValidBrand() {
                return this.isValidVIN && this.selectedBrand;
            },
            isUntouchedVIN() {
                return this.untouchedVIN && this.selectedVIN === '';
            },
            partList() {
                return this.parts
                    .filter(part => part.name !== '')
                    .map ( part => part.name)
                    .join('; ');
            }
        }
    }
</script>

<style scoped>
    .view-container {
        max-width: 600px;
        min-width: 320px;
        padding: 0.2rem 1rem 1rem 1rem;
        margin: 1rem 0;
    }

    .control-label {
        margin: 10px 0 5px 5px;
        text-align: left;
    }
</style>
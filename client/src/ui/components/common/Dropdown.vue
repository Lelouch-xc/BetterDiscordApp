/**
 * BetterDiscord Dropdown Component
 * Copyright (c) 2015-present Jiiks/JsSucks - https://github.com/Jiiks / https://github.com/JsSucks
 * All rights reserved.
 * https://betterdiscord.net
 *
 * This source code is licensed under the MIT license found in the
 * LICENSE file in the root directory of this source tree.
*/

<template>
    <div class="bd-dropdown" :class="{'bd-active': active, 'bd-disabled': disabled}">
        <div class="bd-dropdownCurrent" @click="() => active = !active && !disabled">
            <span class="bd-dropdownText">{{ getSelectedText() }}</span>
            <span class="bd-dropdownArrowWrap">
                <span class="bd-dropdownArrow"></span>
            </span>
        </div>
        <div class="bd-dropdownOptions bd-flex bd-flexCol" ref="options" v-if="active">
            <div class="bd-dropdownOption" v-for="option in options" :class="{'bd-dropdownOptionSelected': value === option.value}" @click="select(option)">{{ option.text }}</div>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['options', 'value', 'disabled'],
        data() {
            return {
                active: false,
                clickHandler: null
            };
        },
        methods: {
            getSelectedText() {
                const selected_option = this.options.find(option => option.value === this.value);
                return selected_option ? selected_option.text : this.value;
            },
            select(option) {
                this.$emit('input', option.value);
                this.active = false;
            }
        },
        mounted() {
            document.addEventListener('click', this.clickHandler = e => {
                let options = this.$refs.options;
                if (options && !options.contains(e.target) && options !== e.target) {
                    this.active = false;
                }
            });
        },
        beforeDestroy() {
            if (this.clickHandler) document.removeEventListener('click', this.clickHandler);
        }
    }
</script>

<template>
    <div class="vsm-item" :class="[{'open-item' : show}, {'active-item' : active}, {'parent-active-item' : childActive}]">
        <template v-if="!item.child">
            <template v-if="isRouterLink">
                <router-link class="vsm-link" :to="item.href" :disabled="item.disabled" :event="item.disabled ? '' : 'click'" @click.native="clickEvent">
                    <i v-if="item.icon" class="vsm-icon" :class="item.icon"></i>
                    <span class="vsm-title">{{item.title}}</span>
                </router-link>
            </template>
            <template v-else>
                <a class="vsm-link" :href="item.href" :disabled="item.disabled" @click="clickEvent">
                    <i v-if="item.icon" class="vsm-icon" :class="item.icon"></i>
                    <span class="vsm-title">{{item.title}}</span>
                </a>
            </template>
        </template>
        <template v-else>
            <a href="#" class="vsm-link" @click.prevent="toggleDropdown">
                <i v-if="item.icon" class="vsm-icon" :class="item.icon"></i>
                <span class="vsm-title">{{item.title}}</span>
                <i class="vsm-arrow" :class="{'open-arrow' : show}"></i>
            </a>
            <div class="vsm-dropdown">
                <transition name="show-animation">
                    <div class="vsm-list" v-if="show">
                        <item v-for="(subItem, index) in item.child" :item="subItem" :key="index" />
                    </div>
                </transition>
            </div>
        </template>
    </div>
</template>

<script>
import Item from './Item.vue'
import { itemMixin } from '../mixin'

export default {
  data() {
    return {
      show: false
    }
  },
  mixins: [itemMixin],
  props: {
    item: Object
  },
  components: {
    Item
  },
  beforeCreate() {
    this.$options.components.Item = require('./Item.vue').default
  }
}
</script>

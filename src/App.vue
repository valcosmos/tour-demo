<script lang="ts" setup>
import { Menu, MenuItem, Dropdown, Button } from 'ant-design-vue'
import { ref, onMounted } from 'vue'
import Driver from 'driver.js'
import 'driver.js/dist/driver.min.css'

const current = ref<string[]>(['mail'])

const visible = ref(false)

const driver = new Driver()

onMounted(async () => {
  visible.value = true
  setTimeout(() => {
    visible.value = false
    tour()
  })
})

function tour() {
  driver.defineSteps([
    {
      element: '#menu',
      popover: {
        className: 'first-step-popover-class',
        title: 'Title on Popover',
        description: 'Body of the popover',
        position: 'bottom'
      },
      onNext: () => {
        visible.value = true
        // Prevent moving to the next step
        driver.preventMove()

        // Perform some action or create the element to move to
        // And then move to that element
        setTimeout(() => {
          driver.moveNext()
        }, 600)
      }
    },
    {
      element: '#submenu',
      popover: {
        title: 'Title on Popover',
        description: 'Body of the popover',
        position: 'bottom'
      },
      onNext: () => {
        visible.value = false
      }
    }
  ])
  driver.start()
}
</script>

<template>
  <div class="box">
    <Menu v-model:selectedKeys="current" mode="horizontal">
      <MenuItem key="mail" id="menu">
        <Dropdown :trigger="['click']" v-model:visible="visible">
          <a class="ant-dropdown-link" id="alink" @click.prevent>Menu1</a>
          <template #overlay>
            <div class="card">
              <Menu>
                <MenuItem>
                  <a href="javascript:;">1st menu item</a>
                </MenuItem>
                <MenuItem id="submenu">
                  <a href="javascript:;">2nd menu item</a>
                </MenuItem>
                <MenuItem>
                  <a href="javascript:;">3rd menu item</a>
                </MenuItem>
              </Menu>
              <Menu>
                <MenuItem>
                  <a href="javascript:;">1st menu item</a>
                </MenuItem>
                <MenuItem>
                  <a href="javascript:;">2nd menu item</a>
                </MenuItem>
                <MenuItem>
                  <a href="javascript:;">3rd menu item</a>
                </MenuItem>
              </Menu>
            </div>
          </template>
        </Dropdown>
      </MenuItem>
      <MenuItem key="app" @click.stop="tour">Start wizard</MenuItem>
    </Menu>
  </div>
</template>

<style>
.box {
  width: 800px;
  margin: 20px auto;
}

.ant-dropdown {
  width: 100%;
  top: 66px !important;
}
</style>

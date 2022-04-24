## 后台管理系统

  某购物商城后台管理系统，包含商品管理，数据统计，订单管理等等信息。 
  
  技术点：Vue + Vue-router  + Vuex + Element-ui + Axios  + Echarts + 其他三方库（wangEditor 富文本编辑器、Vue I18n）
  
  项目搭建：1. vue create vue-ego 
           2. vue-router vuex 
           3. axios 
           4. vue add element --(按需)
  后台服务：1. node.js服务  
            2. express 
            3. jwt（生成token）jsonwebtoken   解析token: 安装： jwt-decode 
            4. mysql
            5. mockjs  -- 模拟数据 
                1. 安装： cnpm i mockjs -S 
                2. 引入： node.js: const Mock = require('mockjs')
                 前端js:  import Mock from 'mockjs'
               3. 语法：Mock.mock() 
  富文本编辑：wangEditor 
             1. 安装：npm i wangeditor --save 
             2. 引入模块：import E from "wangeditor"
             3. 使用wangeditor
                    const editor = new E("#div1")
                    editor.create()
  vuei8n ：1. Vue I18n 是 Vue.js 的国际化插件。它可以轻松地将一些本地化功能集成到你的 Vue.js 应用程序中。
           2.安装：1. npm install vue-i18n
                   2. main.js导入或者是单独的文件
                      import Vue from 'vue'
                      import VueI18n from 'vue-i18n'
                      Vue.use(VueI18n)
           3.使用：1）如果使用模块系统 (例如通过 vue-cli)，则需要导入 Vue 和 VueI18n ，然后调用 Vue.use(VueI18n)。
                   // import Vue from 'vue'
                   // import VueI18n from 'vue-i18n'
                   // Vue.use(VueI18n)
                   2）准备翻译的语言环境信息
         const messages = {
            en: {//英文
                home: {
                    hello: 'hello world',
                    xx:xx,
                    ...
                },
                goods:{

                }
            },
            zh: {//中文
                home: {
                    hello: '你好 世界',
                    xx:xx,
                    ...
                },
                goods:{
                    
                }
            }
        }
                 3）通过选项创建 VueI18n 实例
                const i18n = new VueI18n({
                     locale: 'en', // 设置地区
                     messages, // 设置地区信息
                })
                4）通过 `i18n` 选项创建 Vue 实例
                   new Vue({ i18n }).$mount('#app')
                5）使用语法：<p>{{ $t("home.hello") }}</p>
  element 国际化：1. 导入 import Element from 'element-ui'
                  2. 导入语言环境
                     import enLocale from 'element-ui/lib/locale/lang/en'
                     import zhLocale from 'element-ui/lib/locale/lang/zh-CN'
                  3. 配置语言环境
                     const messages = {
                        en: {
                            message: 'hello',
                            ...enLocale // 或者用 Object.assign({ message: 'hello' }, enLocale)
                        },
                       zh: {
                           message: '你好',
                       ...zhLocale // 或者用 Object.assign({ message: '你好' }, zhLocale)
                      }
                    }
                 4. 配置使用
                   Vue.use(Element, {
                       i18n: (key, value) => i18n.t(key, value)
                   })


vuejs or element ui use problem address

使用拓展运算符 ... 复制数组

// good
itemsCopy = [...items]

// better
function getFullName ({ firstName, lastName }) {
  return `${firstName} ${lastName}`
}


程序化生成字符串时，请使用模板字符串
const str = `ab${test}`

不要在非函数代码块中声明函数
// good
let test
if (isUse) {
  test = () => {
    // do something
  }
}

不要更改函数参数的值
function test (opts = {}) {
  // ...
}

使用标准的 ES6 模块语法 import 和 export
// better
import { Util } from './util'
export default Util

es6数组去重的两种方式

1.
let sq = ['aa','cc','bb','aa','33','bb'];
let news = new Set(sq);
console.log(news);
console.log([...news]);

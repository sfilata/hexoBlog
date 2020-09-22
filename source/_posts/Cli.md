---
title: 一个提升开发效率的CLI工具
date: 2020-03-31 15:17:55
tags: 技术
categories: 编程
---
##### 用到的工具：
commander.js

##### 安装
``` shell
npm install command --save
```

##### 实现
首先编写一个可执行的脚本文件，定为*init-project.js*
```javascript
#!/usr/bin/env node

/**
* Module dependency
*/

const program = require('commander')
const path = require('path')
const vfs = require('vinyl-fs')
const chalk = require('chalk')
const fs = require('fs')

function pathMapObj(type, param) {
    return {
        'index': {
            input: './demo/index.vue',
            output: path.resolve(`./src/entries/${type}/pages/${param}`)
        },
        'actions': {
            input: './demo/action/*',
            output: path.resolve(`./src/entries/${type}/store/${param}`)
        },
        'api': {
            input: './demo/api.js',
            output: path.resolve(`./src/api/${type}`)
        }
    }
}

/**
*
* @param {输入路径} input
* @param {输出路径} output
* @param {类型} type
* @param {是否需要重命名} needRename
* @param {模块名} moduleName
*/

function readAndWrite(input, output, type, needRename, moduleName) {
    vfs.src([input])
    .pipe(vfs.dest(output, {}))
    .on('end', function () {
        console.log(chalk.green(`initializing ${type} file finished......`))
        if (needRename) {
            fs.renameSync(`${output}/api.js`, `${output}/${moduleName}.js`)
        }
    })
    .resume()
}
program.version('1.0.0')
    .option('-t --teacher [name]', 'init the teacher component')
    .option('-s --student [name]', 'init the student component')
    .option('-m --manager [name]', 'init the manager component')
    .parse(process.argv)
let param = program.teacher || program.student || program.manager
if (param) {
    let type = null;
    if (program.teacher) {
        type = 'teacher'
    } else if (program.student) {
        type = 'student'
    } else if (program.manager) {
        type = 'manager'
    }
    if (param.toString() === 'true') {
        console.log(chalk.red('格式错误！缺少组件名参数'))
        return
    }
    console.log(`The ${chalk.blue(param)} component will be inited on ${chalk.yellow(__dirname)}`)
    console.log(chalk.yellow('initializing specified component......'))\

    readAndWrite(pathMapObj(type, param).index.input, pathMapObj(type, param).index.output, 'index')
    readAndWrite(pathMapObj(type, param).actions.input, pathMapObj(type, param).actions.output, 'actions')
    readAndWrite(pathMapObj(type, param).api.input, pathMapObj(type, param).api.output, 'api', true, param)

} else {
    console.log(chalk.red('格式错误！正确命令举例：node ./bin/init-project -t component-name'))
}
```
上面文件主要功能为读取demo文件夹的样例模板文件，并按照类型和模块名进行相应路径的输出，以达到自动初始化模块开发的功能，免去每次开发新功能时创建多个重复文件的苦恼

windows下存在问题，不识别脚本声明语句`#!/usr/bin/env node`，导致只能使用node来执行代码

在同级demo文件夹下放入要生成的样例文件。目前有四个文件，主要页面vue文件，action与store vuex所需文件，api（使用axio进行访问）文件。
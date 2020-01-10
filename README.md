# Wiki1001-Template
The Template For Wiki1001

## Build Setup  ##
```bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```
##  Config Modify   ##
**filePath**：...\docs\.vuepress\theme\components\Page.vue
**codeAnchor**：script/methods/valine()/new Valine()
```vue
appId: '****************************',// your appId
appKey: '***********************', // your appKey
```
**filePath**：...\docs\.vuepress\config.js
```vue
//	浏览器页签常量标题
title: 'Wiki 1001',
//	显示在<meta>标签的页面描述
description: '***的 *** 维基博客',
//  指定 vuepress build 的输出目录
dest:'./dist',
// 注入到当前页面的 HTML <head> 中的标签
head: [
 // 增加一个自定义的 favicon(网页标签的图标)
['link', { rel: 'icon', href: '/img/logo.ico' }],
],
// 这是部署到github的路径配置
base: '/Wiki1001Pro/', 
...
plugins：[
...
['@vuepress/google-analytics', {
	//  谷歌ga ID
	ga:'**-*********-*'
}],
...
]
themeConfig: {
	// 假定是 GitHub. 同时也可以是一个完整的 GitLab URL
	repo: 'https://github.com/Mulander-J/Wiki1001Pro.git',
	// 自定义仓库链接文字。默认从 `themeConfig.repo` 中自动推断为"GitHub"/"GitLab"/"Bitbucket" 其中之一，或是 "Source"。
	repoLabel: 'GitHub',
	// 以下为可选的编辑链接选项,假如你的文档仓库和项目本身不在一个仓库,可以在此区分链接：
	docsRepo: 'https://github.com/Mulander-J/Wiki1001Pro',
	// 假如文档不是放在仓库的根目录下：
	docsDir: 'docs',
	// 假如文档放在一个特定的分支下：
	docsBranch: 'master',
	// 默认是 false, 设置为 true 来启用
	editLinks: true,
	// 默认为 "Edit this page"
	editLinkText: 'Git EditLink',
	// 文档更新时间：每个文件git最后提交的时间,
	lastUpdated: 'Last Updated' ,
	// 侧边栏搜索深度
	sidebarDepth: 2, // 将同时提取markdown中h2 和 h3 标题，显示在侧边栏上。
	...
}
```



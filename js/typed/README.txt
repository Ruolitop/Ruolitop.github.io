typed.js —— 副标题打字机效果（自托管）

  文件    : typed.umd.min.js
  版本    : 3.0.0
  来源    : https://cdn.jsdelivr.net/npm/typed.js@3.0.0/dist/typed.umd.min.js
  许可证  : MIT（见同目录 LICENSE.txt，Copyright (c) 2018 Matt Boldt）

为什么要自托管
--------------
主题 _config.butterfly.yml → subtitle.effect: true 时，模板
layout/includes/third-party/subtitle.pug 会执行：

    btf.getScript('!{url_for(theme.asset.typed)}').then(subtitleType)

即「先取 typed.js，成功后再渲染副标题」。默认 asset.typed 指向
jsDelivr（plugins.yml: typed.js@3.0.0），而这里**没有 catch**：
一旦该请求失败，.then() 就不会执行，副标题永远保持空白（而不是回退到
sub 的兜底文字），且控制台不会报错。

node_modules/hexo-theme-butterfly/scripts/events/cdn.js 里：

    themeConfig.asset = Object.assign(..., deleteNullValue(CDN.option))

即 CDN.option 的键会直接覆盖 asset 的同名键。所以改为本机文件只需在
_config.butterfly.yml 的 CDN.option 下加一行：

    typed: /js/typed/typed.umd.min.js

与本项目已自托管的 KaTeX CSS/JS、algoliasearch-lite 保持同一策略。

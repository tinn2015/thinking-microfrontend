# 前端微服务

   微前端架构是一种类似于微服务的架构，它将微服务的理念应用于浏览器端，即将 Web 应用由单一的单体应用转变为多个小型前端应用聚合为一的应用。
由此带来的变化是，这些前端应用可以独立运行、独立开发、独立部署。以及，它们应该可以在共享组件的同时进行并行开发——这些组件可以通过 NPM 或者 Git Tag、Git Submodule 来管理。

微前端架构一般可以由以下几种方式进行：

1. 使用 HTTP 服务器的路由来重定向多个应用

2. 在不同的框架之上设计通讯、加载机制，诸如 Mooa 和 Single-SPA

3. 通过组合多个独立应用、组件来构建一个单体应用

4. iFrame。使用 iFrame 及自定义消息传递机制

5. 使用纯 Web Components 构建应用

6. 结合 Web Components 构建

## 微服务架构带来了哪些好处？
假设服务边界已经被正确地定义为可独立运行的业务领域，并确保在微服务设计中遵循诸多最佳实践。那么至少会在以下几个方面获得显而易见的好处：

复杂性：服务可以更好地分离，每一个服务都足够小，能够完成完整的定义清晰的职责；
扩展性：每一个服务可以独立横向扩展以满足业务伸缩性，并减少资源的不必要消耗；
灵活性：每一个服务可以独立失败，允许每个团队自主选择最适合他们的技术和基础架构；
敏捷性：每一个服务都可以独立开发，测试和部署，并允许团队独立扩展和维护各自的部署服务。

微前端（Micro Frontends）这个术语其实就是微服务的衍生物。将微服务理念扩展到前端开发，同时构建多个完全自治、松耦合的 App 模块（服务），其中每个 App 模块只负责特定的 UI 元素和功能。

如果我们看到微服务提供给后端的好处，就可以更进一步将这些好处应用到前端。与此同时，在设计微服务的时候，就可以考虑不仅要完成后端逻辑，而且还要完成前端的视觉部分。而微前端与微服务的许多要求也是一致的：监控、日志、HealthCheck、Analytics 等等。

Be Technology Agnostic：每个团队都应该能够选择并升级他们的技术栈，而不必与其他团队协调。自定义元素（后面会具体提到）是隐藏实现细节的好方法，同时为其他人提供公共接口。
Isolate Team Code：即使所有团队使用相同的框架，也不要共享运行时。构建独立的应用程序。不要依赖共享状态或全局变量。
Establish Team Prefixes：相互约定命名隔离。为 CSS、浏览器事件、Local Storage 和 Cookies 制定命名空间，以避免冲突，明确其所有权。
Favor Native Browser Features over Custom APIs：使用浏览器事件进行通信，而不是构建全局的 PubSub 系统。如果确实需要构建跨团队 API，请尽量保持简单。（与框架无关，可使用 CustomEvent）
Build a Resilient Site：即使 JavaScript 失败或尚未执行，Web 应用程序的功能仍应有效。可以使用通用渲染和渐进增强来提高用户的感知性能。

## 拆分微前端所带来的好处
拆分微前端能使各个前端团队按照自己的步调进行迭代，并随时准备就绪进入可发布状态，并隔离相互依赖所产生的风险，与此同时也更容易尝试新技术。

Web 应用程序被分解成独立的特征，并且每个特征都由不同的团队拥有，前端到后端。这确保了每个功能都是独立于其他功能开发、测试和部署的。
将网站或 Web 应用程序视为由独立团队拥有的功能组合。每个团队都有一个独特的业务或关注点确定的任务。
每一个团队是跨职能的，从数据库到用户界面端到端地开发其功能/特性。
所有前端功能（身份验证，库存，购物车等）都是 Web 应用程序的一部分，并与后端（大部分时间通过 HTTP）进行通信，并将其分解为微服务。
可以同时拥有后端、前端、数据访问层和数据库，即一个服务子域所需的所有内容。
查找线上 bug、测试、框架迭代，甚至语言、代码隔离与责任和其他事情变得更容易处理。
我们不得不付出的代价是部署，但是容器（Docker 和 Rocket）以及不可变服务器使得这种情况也得到了极大的改善。


## 使用 iFrame 创建容器

iframe 可以创建一个全新的独立的宿主环境，这意味着我们的前端应用之间可以相互独立运行。采用 iframe 有几个重要的前提：

网站不需要 SEO 支持

拥有相应的应用管理机制。

如果我们做的是一个应用平台，会在我们的系统中集成第三方系统，或者多个不同部门团队下的系统，显然这是一个不错的方案。
如果这一类应用过于复杂，那么它必然是要进行微服务化的拆分。因此，在采用 iframe 的时候，我们需要做这么两件事：

* 设计管理应用机制

* 设计应用通讯机制
**加载机制**在什么情况下，我们会去加载、卸载这些应用；在这个过程中，采用怎样的动画过渡，让用户看起来更加自然。
**通讯机制**。直接在每个应用中创建 postMessage 事件并监听，并不是一个友好的事情。其本身对于应用的侵入性太强，因此通过 iframeEl.contentWindow 去获取 iFrame 元素的 Window 对象是一个更简化的做法。随后，就需要定义一套通讯规范：事件名采用什么格式、什么时候开始监听事件等等

采用iframe 的微前端框架 Mooa, Single-spa

## Conponents

<link rel="import" href="/components/microfrontends/header.html">
<link rel="import" href="/components/microfrontends/products-list.html">
<link rel="import" href="/components/microfrontends/cart.html">



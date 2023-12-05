1、**RESTful API 基本原则**: 请解释什么是RESTful API以及它的六个基本原则（也称为约束）。
restful api是一种架构风格，用来设计网络应用程序的接口。
它有6个基本原则：统一接口、无状态、可缓存、客户端-服务器分离、分层系统和按需代码
![[Pasted image 20231119092700.png]]
![[Pasted image 20231119092724.png]]
![[Pasted image 20231119111158.png]]

![[Pasted image 20231119111220.png]]
2、**HTTP 方法**: RESTful API通常使用哪些HTTP方法？每种方法的用途是什么？
GET（获取资源）、POST（创建资源）、PUT（更新资源）、DELETE（删除资源）、PATCH（部分更新资源）

3、**状态码**: 常见的HTTP状态码有哪些？它们各自代表什么意思？

常见的HTTP状态码有200（OK）、201（已创建）、204（无内容）、400（错误请求）、401（未授权）、403（禁止访问）、404（未找到）、500（服务器内部错误）等。
![[Pasted image 20231119111439.png]]
![[Pasted image 20231119111454.png]]
![[Pasted image 20231119111509.png]]
![[Pasted image 20231119111529.png]]
4、**URI 设计**: 一个良好设计的RESTful API的URI应该遵循哪些最佳实践？
![[Pasted image 20231119111803.png]]
![[Pasted image 20231119111825.png]]
![[Pasted image 20231119111905.png]]
![[Pasted image 20231119112404.png]]
![[Pasted image 20231119112454.png]]
![[Pasted image 20231119112515.png]]
![[Pasted image 20231119112610.png]]
![[Pasted image 20231119112831.png]]
![[Pasted image 20231119112847.png]]

5、**无状态性**: 解释RESTful API的无状态性及其对API设计的影响。

无状态意味着每个HTTP请求都应包含执行该请求所需的所有信息，而不依赖于服务器的上下文或状态。
![[Pasted image 20231119114718.png]]
![[Pasted image 20231119114743.png]]
![[Pasted image 20231119120029.png]]
![[Pasted image 20231119122637.png]]
![[Pasted image 20231119122704.png]]
![[Pasted image 20231119123458.png]]
![[Pasted image 20231119123517.png]]

6、**认证与授权**: 你会如何在RESTful API中实现认证和授权？
我会通过OAuth、JWT（JSON Web Tokens）等标准化的认证和授权机制来实现安全控制。
![[Pasted image 20231119124937.png]]

![[Pasted image 20231119125030.png]]
![[Pasted image 20231119125100.png]]
![[Pasted image 20231119125136.png]]
![[Pasted image 20231119125205.png]]
![[Pasted image 20231119125345.png]]
![[Pasted image 20231119125404.png]]
![[Pasted image 20231119125428.png]]
![[Pasted image 20231119130035.png]]
![[Pasted image 20231119130055.png]]
![[Pasted image 20231119130117.png]]

7、**版本控制**: 如何在RESTful服务中管理API版本？

我倾向于在HTTP头部中处理版本控制，或者通过URI路径来指定版本。
![[Pasted image 20231119132440.png]]
![[Pasted image 20231119132458.png]]
![[Pasted image 20231119132514.png]]
![[Pasted image 20231119132550.png]]
![[Pasted image 20231119132648.png]]
![[Pasted image 20231119133046.png]]
![[Pasted image 20231119133101.png]]
![[Pasted image 20231119133146.png]]
![[Pasted image 20231119133224.png]]
![[Pasted image 20231119133404.png]]
![[Pasted image 20231119133451.png]]
![[Pasted image 20231119133506.png]]
![[Pasted image 20231119133657.png]]
![[Pasted image 20231119133726.png]]
![[Pasted image 20231119134021.png]]
![[Pasted image 20231119134121.png]]
![[Pasted image 20231119134141.png]]
![[Pasted image 20231119134154.png]]
![[Pasted image 20231119134210.png]]
![[Pasted image 20231119134223.png]]
![[Pasted image 20231119134236.png]]

8、**错误处理**: 在RESTful API中，应该如何处理和传递错误信息？

错误应以标准化的格式返回，通常包含错误码、错误信息和可能的解决方案。
![[Pasted image 20231119134941.png]]
![[Pasted image 20231119135002.png]]
![[Pasted image 20231119135021.png]]
![[Pasted image 20231119135053.png]]
![[Pasted image 20231119135123.png]]
全局异常处理类
![[Pasted image 20231119135211.png]]
您可能需要为不同类型的错误条件创建多个自定义异常，并在全局异常处理器中相应地处理它们。这不仅使错误处理更加集中和一致，而且也使得控制器代码保持清晰和易于维护。

9、**性能优化**: 可以采取哪些措施来提高RESTful API的性能？

通过使用缓存、限制数据大小、分页、选择合适的数据格式等方法来提高性能。
![[Pasted image 20231119135825.png]]
![[Pasted image 20231119135839.png]]
![[Pasted image 20231119135851.png]]
![[Pasted image 20231119135902.png]]
![[Pasted image 20231119135915.png]]
关于第5步继续展开来说是
![[Pasted image 20231119140400.png]]
![[Pasted image 20231119140418.png]]
![[Pasted image 20231119140831.png]]
![[Pasted image 20231119140852.png]]
![[Pasted image 20231119140915.png]]
![[Pasted image 20231119140940.png]]
![[Pasted image 20231119153929.png]]
![[Pasted image 20231119153946.png]]
![[Pasted image 20231119154001.png]]
![[Pasted image 20231119154017.png]]
![[Pasted image 20231119154036.png]]
![[Pasted image 20231119154049.png]]


![[Pasted image 20231119135935.png]]

10、**幂等性**: 什么是幂等性？哪些HTTP方法应该是幂等的？

幂等性是指无论操作进行多少次，资源的状态都是一致的。GET、PUT、DELETE是幂等的HTTP方法。

11、**缓存**: RESTful API如何利用缓存来提高响应速度？

可以通过HTTP头部中的缓存控制指令来实现缓存，以减少重复数据的传输。

12、**HATEOAS**: 你对HATEOAS（Hypermedia As The Engine Of Application State）的理解是什么？它在实际应用中如何实现？

HATEOAS是一种约束，它使得客户端可以通过服务器提供的动态链接来发现所有可用的动作和资源。

13、**内容协商**: 什么是内容协商，RESTful API是如何实现的？

内容协商是指客户端和服务器通过HTTP头部协商返回哪种格式的资源数据，如JSON或XML。

14、**API文档**: 描述一下你如何编写和维护API文档。

我会使用Swagger或OpenAPI规范来创建自动化的、可浏览的API文档，这些文档是可交互的，可以直接从文档中尝试API调用。

------------------------------------

1、rest即表现层状态转移，是一种在web上提供标准的架构风格，使系统之间更容易通信。符合rest标准的系统通常被称为restful系统，
其特点是无状态并将客户端和服务器的关注点分离。
我们将详细介绍这些术语的含义以及他们对web服务的有益特性。

2、在rest架构风格中，客户端的实现和服务器的实现可以独立进行，彼此不知道对方的存在。这意味着客户端代码可以随意更改而不影响服务器的运行，服务器端的代码也可以更改而不影响客户端的运行。

3、只要双方知道向对方发送的消息格式，它们可以保持模块化和分离。通过将用户界面关注点与数据存储关注点分离，我们提高了界面在各个平台上的灵活性，并通过简化服务器组件来提高可扩展性。此外，分离使得每个组件都能够独立演化。

4、通过使用rest接口，不同的客户端访问相同的rest端点，执行相同的操作，并接受相同的响应。

5、遵循rest范式的系统是无状态的，这意味着服务器不需要知道客户端处于什么状态，反之亦然。这样，服务端和客户端都可以理解接收到的任何信息。即使没有看到之前的消息。这种无状态的约束是通过使用资源而不是使用命令来实现。资源是web的名词-它们描述了您可能需要存储或发送给其他服务的任何对象、文档或事务。

6、rest系统通过对资源进行标准操作进行交互，它们不依赖于接口的实现。

7、这些约束帮助restful应用程序实现可靠性、快速性能和可扩展性。因为组件可以在系统运行期间进行管理、更新和重用，而不会影响整个系统。

8、实现restful 接口时客户端和服务器之间的通信实际上是如何发生的？

rest要求客户端对服务器发出请求，以检索或修改服务器上的数据。请求通常包括：
1、一个http动词，用于定义要执行的操作类型。
2、一个头部，允许客户端传递有关请求的信息。
3、资源的路径
4、一个可选的消息正文，包含数据。

![[Pasted image 20231119065321.png]]

在请求的头部，客户端发送能够接收服务器发送的内容的类型。这被称为 `Accept` 字段，并确保服务器不会发送客户端无法理解或处理的数据。内容类型的选项是MIME类型（或多用途互联网邮件扩展），您可以在MDN Web文档中了解更多信息。

MIME类型，用于指定 `Accept` 字段中的内容类型，由 `type` 和 `subtype` 组成。它们之间用斜杠（/）分隔。

例如，包含HTML的文本文件将使用类型 `text/html` 指定。如果这个文本文件包含CSS，它将被指定为 `text/css` 。通用文本文件将被表示为 `text/plain` 。然而，默认值 `text/plain` 并不是一个万能的值。如果客户端期望 `text/css` ，但接收到 `text/plain` ，它将无法识别内容。

客户端访问服务器上的一个资源，其中 `id` 23在 `articles` 资源中，可能会发送如下的GET请求：

```
GET /articles/23
Accept: text/html, application/xhtml
```

在这种情况下， `Accept` 头字段表示客户端将接受 `text/html` 或 `application/xhtml` 中的内容。

请求必须包含一个操作应该执行的资源路径。在RESTful API中，路径应该被设计成帮助客户端了解正在发生的事情。

按照惯例，路径的第一部分应该是资源的复数形式。这样可以使嵌套路径易于阅读和理解。

一个像 `fashionboutique.com/customers/223/orders/12` 这样的路径，无论你以前是否见过这个具体的路径，它指向的内容是清晰的，因为它是分层和描述性的。我们可以看到我们正在访问具有 `id` 12的订单，该订单是属于 `id` 223的客户的。

路径应包含定位资源所需的信息，具体程度取决于需要。当引用资源列表或集合时，不总是需要添加一个 `id` 。例如，对于 `fashionboutique.com/customers` 路径的POST请求不需要额外的标识符，因为服务器将为新对象生成一个 `id`

如果我们想要访问单个资源，我们需要在路径后面添加 `id` 。例如： `GET fashionboutique.com/customers/:id` - 检索具有指定 `id` 的 `customers` 资源中的项目。 `DELETE fashionboutique.com/customers/:id` - 删除具有指定 `id` 的 `customers` 资源中的项目。

在服务器向客户端发送数据有效载荷的情况下，服务器必须在响应的头部中包含一个 `content-type` 。这个 `content-type` 头字段通知客户端响应体中发送的数据类型。这些内容类型是MIME类型，就像请求头的 `accept` 字段中一样。服务器在响应中发送回的 `content-type` 应该是客户端在请求的 `accept` 字段中指定的选项之一。

例如，当客户端使用GET请求访问具有 `id` 23 的 `articles` 资源时：

```
GET /articles/23 HTTP/1.1
Accept: text/html, application/xhtml
```

服务器会返回这样的响应头

```
HTTP/1.1 200 (OK)
Content-Type: text/html
```

![[Pasted image 20231119070048.png]]

![[Pasted image 20231119070108.png]]
![[Pasted image 20231119070254.png]]
![[Pasted image 20231119070315.png]]

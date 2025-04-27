# 论坛系统API
基于Gin框架的论坛系统API

## Version: 1.0

### Terms of service
http://swagger.io/terms/

**Contact information:**  
API Support  
http://www.example.com/support  
support@example.com  

**License:** [MIT](https://opensource.org/licenses/MIT)

[OpenAPI](https://swagger.io/resources/open-api/)

### Security
**BearerAuth**  

| apiKey | *API Key* |
| ------ | --------- |
| Description | 使用Bearer token进行身份验证 |
| Name | Authorization |
| In | header |

---
### /admin/comments

#### GET
##### Summary

获取评论列表 (管理员)

##### Description

管理员获取所有评论列表，支持分页、排序和筛选。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认10 | No | integer |
| sort_by | query | 排序字段 (e.g., created_at) | No | string |
| order | query | 排序方式 (asc, desc) | No | string |
| search | query | 搜索关键词 (匹配内容) | No | string |
| post_id | query | 帖子ID筛选 | No | integer |
| author_id | query | 作者ID筛选 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回评论列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.CommentListItem](#responsecommentlistitem) ] } |
| 400 | 无效的请求参数 | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 无权访问（需要管理员权限） | [response.ErrorResponse403](#responseerrorresponse403) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

---
### /admin/posts

#### GET
##### Summary

获取帖子列表 (管理员)

##### Description

管理员获取所有帖子列表，支持分页、排序和筛选。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认10 | No | integer |
| sort_by | query | 排序字段 (e.g., created_at, view_count, like_count) | No | string |
| order | query | 排序方式 (asc, desc) | No | string |
| search | query | 搜索关键词 (匹配标题、内容) | No | string |
| status | query | 帖子状态筛选 (e.g., published, hidden, pinned, featured) | No | string |
| author_id | query | 作者ID筛选 | No | integer |
| category_id | query | 分类ID筛选 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回帖子列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.PostListResponse](#responsepostlistresponse) ] } |
| 400 | 无效的请求参数 | [response.Response](#responseresponse) |
| 401 | 未登录或认证令牌无效 | [response.Response](#responseresponse) |
| 403 | 无权访问（需要管理员权限） | [response.Response](#responseresponse) |
| 500 | 服务器内部错误 | [response.Response](#responseresponse) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

---
### /admin/users

#### GET
##### Summary

获取所有用户信息

##### Description

返回系统中所有用户的列表，支持分页、排序和筛选。需要管理员权限。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认10 | No | integer |
| sort_by | query | 排序字段 (created_at, nickname, last_login) | No | string |
| order | query | 排序方式 (asc, desc) | No | string |
| search | query | 搜索关键词 (匹配用户名、昵称、邮箱) | No | string |
| status | query | 用户状态筛选 (active, inactive, banned) | No | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回用户列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.UserListInfo](#responseuserlistinfo) ] } |
| 400 | 无效的请求参数 | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 无权访问（需要管理员权限） | [response.ErrorResponse403](#responseerrorresponse403) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /admin/users/{id}/role

#### PUT
##### Summary

更新用户角色 (管理员)

##### Description

管理员更新指定用户的角色。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 用户ID | Yes | integer |
| request | body | 更新角色请求参数 (role: 'admin', 'moderator', 'user') | Yes | [request.UpdateUserRoleRequest](#requestupdateuserrolerequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 更新成功，返回新的角色信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.UpdateUserRoleResponse](#responseupdateuserroleresponse) } |
| 400 | 无效的用户ID或角色值 | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 无权访问（需要管理员权限） | [response.ErrorResponse403](#responseerrorresponse403) |
| 404 | 用户不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /admin/users/{id}/status

#### PUT
##### Summary

更新用户状态 (管理员)

##### Description

管理员更新指定用户的状态（如禁用、启用）。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 用户ID | Yes | integer |
| request | body | 更新状态请求参数 (status: 'active', 'inactive', 'banned') | Yes | [request.UpdateUserStatusRequest](#requestupdateuserstatusrequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 更新成功，返回新的状态信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.UpdateUserStatusResponse](#responseupdateuserstatusresponse) } |
| 400 | 无效的用户ID或状态值 | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 无权访问（需要管理员权限） | [response.ErrorResponse403](#responseerrorresponse403) |
| 404 | 用户不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

---
### /categories

#### GET
##### Summary

获取分类列表

##### Description

获取所有分类的列表，支持分页。返回分类的基本信息，包括ID、名称、描述、父分类ID和帖子数量。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认20 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回分类列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.CategoryResponse](#responsecategoryresponse) ] } |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

#### POST
##### Summary

创建分类

##### Description

创建新的分类，需要提供分类名称、描述和可选的父分类ID。只有管理员可以创建分类。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| request | body | 创建分类请求参数 | Yes | [request.CreateCategoryRequest](#requestcreatecategoryrequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 201 | 创建成功，返回新创建的分类信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.CategoryResponse](#responsecategoryresponse) } |
| 400 | 无效的请求参数，可能是分类名称太短或描述不足" example({"code":400,"message":"无效的请求参数"}) | [response.Response](#responseresponse) |
| 401 | 未登录或认证令牌无效" example({"code":401,"message":"未登录或认证令牌无效"}) | [response.Response](#responseresponse) |
| 403 | 没有创建分类的权限（需要管理员权限）" example({"code":403,"message":"需要管理员权限"}) | [response.Response](#responseresponse) |
| 409 | 分类名称已存在" example({"code":409,"message":"分类名称已存在"}) | [response.Response](#responseresponse) |
| 500 | 服务器内部错误" example({"code":500,"message":"服务器内部错误"}) | [response.Response](#responseresponse) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /categories/popular

#### GET
##### Summary

获取热门分类

##### Description

获取帖子数量最多的热门分类，按照帖子数量降序排序。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| limit | query | 数量限制，默认10 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回热门分类列表 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [ [response.CategoryResponse](#responsecategoryresponse) ] } |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

### /categories/{id}

#### GET
##### Summary

获取分类详情

##### Description

根据分类ID获取分类的详细信息，包括名称、描述、父分类ID和帖子数量。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 分类ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回分类详细信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.CategoryResponse](#responsecategoryresponse) } |
| 400 | 无效的分类ID" example({"code":400,"message":"无效的分类ID"}) | [response.Response](#responseresponse) |
| 404 | 分类不存在" example({"code":404,"message":"分类不存在"}) | [response.Response](#responseresponse) |
| 500 | 服务器内部错误" example({"code":500,"message":"服务器内部错误"}) | [response.Response](#responseresponse) |

#### PUT
##### Summary

更新分类

##### Description

更新现有分类的信息，包括名称和描述。只有管理员可以更新分类。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 分类ID | Yes | integer |
| request | body | 更新分类请求参数 | Yes | [request.UpdateCategoryRequest](#requestupdatecategoryrequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 更新成功，返回更新后的分类信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.CategoryResponse](#responsecategoryresponse) } |
| 400 | 无效的请求参数或分类ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 没有更新分类的权限（需要管理员权限） | [response.ErrorResponse403](#responseerrorresponse403) |
| 404 | 分类不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 409 | 分类名称已存在 | [response.ErrorResponse409](#responseerrorresponse409) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

#### DELETE
##### Summary

删除分类

##### Description

删除指定的分类。只有管理员可以删除分类。删除分类会导致该分类下的所有帖子被移动到默认分类。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 分类ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 删除成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的分类ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 没有删除分类的权限（需要管理员权限） | [response.ErrorResponse403](#responseerrorresponse403) |
| 404 | 分类不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /categories/{id}/subcategories

#### GET
##### Summary

获取子分类

##### Description

获取指定分类的所有子分类。返回子分类的基本信息，包括ID、名称、描述、父分类ID和帖子数量。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 父分类ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回子分类列表 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [ [response.CategoryResponse](#responsecategoryresponse) ] } |
| 400 | 无效的分类ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 404 | 父分类不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

---
### /comments

#### POST
##### Summary

创建评论

##### Description

为帖子创建新评论。用户必须登录才能创建评论。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| request | body | 创建评论请求参数 | Yes | [request.CreateCommentRequest](#requestcreatecommentrequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 201 | 创建成功，返回新创建的评论信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.CommentResponse](#responsecommentresponse) } |
| 400 | 无效的请求参数，可能是帖子ID无效或内容不符合要求 | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /comments/posts/{id}

#### GET
##### Summary

获取帖子评论

##### Description

获取指定帖子的所有评论列表，支持分页。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认20 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回评论列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.CommentResponse](#responsecommentresponse) ] } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

### /comments/{id}

#### GET
##### Summary

获取评论详情

##### Description

根据评论ID获取评论的详细信息。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 评论ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回评论详细信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.CommentResponse](#responsecommentresponse) } |
| 400 | 无效的评论ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 404 | 评论不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

#### PUT
##### Summary

更新评论

##### Description

更新现有评论的内容。只有评论作者才能更新评论。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 评论ID | Yes | integer |
| request | body | 更新评论请求参数 | Yes | [request.UpdateCommentRequest](#requestupdatecommentrequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 更新成功，返回更新后的评论信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.CommentResponse](#responsecommentresponse) } |
| 400 | 无效的请求参数或评论ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 无权修改该评论（非作者） | [response.ErrorResponse403](#responseerrorresponse403) |
| 404 | 评论不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

#### DELETE
##### Summary

删除评论

##### Description

删除指定的评论。只有评论作者才能删除评论。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 评论ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 删除成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的评论ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 无权删除该评论（非作者） | [response.ErrorResponse403](#responseerrorresponse403) |
| 404 | 评论不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /comments/{id}/replies

#### GET
##### Summary

获取评论回复

##### Description

获取指定评论的所有回复列表，支持分页。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 评论ID | Yes | integer |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认20 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回回复列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.CommentResponse](#responsecommentresponse) ] } |
| 400 | 无效的评论ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 404 | 评论不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

### /comments/{id}/reply

#### POST
##### Summary

回复评论

##### Description

回复现有评论，创建一个新的子评论。用户必须登录才能回复评论。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 父评论ID | Yes | integer |
| request | body | 回复评论请求参数 | Yes | [request.ReplyCommentRequest](#requestreplycommentrequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 201 | 回复成功，返回新创建的回复信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.CommentResponse](#responsecommentresponse) } |
| 400 | 无效的请求参数，可能是评论ID无效或内容不符合要求 | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 404 | 父评论不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /users/{id}/comments

#### GET
##### Summary

获取用户评论列表

##### Description

获取某个用户发布的所有评论，支持分页。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 用户ID | Yes | integer |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认10 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回评论列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.CommentListItem](#responsecommentlistitem) ] } |
| 400 | 无效的用户ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 404 | 用户不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

---
### /comments/{id}/like

#### POST
##### Summary

点赞评论

##### Description

对指定评论进行点赞。用户必须登录才能点赞。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 评论ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 点赞成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的评论ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 404 | 评论不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 409 | 已经点赞过该评论 | [response.ErrorResponse409](#responseerrorresponse409) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

#### DELETE
##### Summary

取消点赞评论

##### Description

取消对指定评论的点赞。用户必须登录才能取消点赞。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 评论ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 取消点赞成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的评论ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 404 | 评论不存在或未点赞过该评论 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

---
### /favorites/check/{id}

#### GET
##### Summary

检查是否已收藏

##### Description

检查当前登录用户是否已经收藏指定帖子。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 检查成功，返回是否已收藏 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.CheckFavoriteResponse](#responsecheckfavoriteresponse) } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /favorites/count/{id}

#### GET
##### Summary

获取帖子收藏数

##### Description

获取指定帖子的收藏数量。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回收藏数量 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.CountResponse](#responsecountresponse) } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /favorites/post/{id}

#### GET
##### Summary

获取帖子收藏列表

##### Description

获取指定帖子的所有收藏用户，支持分页。后续将完善返回内容，提供帖子标题和用户昵称信息。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认10 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回收藏列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.FavoriteResponse](#responsefavoriteresponse) ] } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /favorites/user

#### GET
##### Summary

获取用户收藏列表

##### Description

获取当前登录用户的所有收藏帖子，支持分页。后续将完善返回内容，提供帖子标题和用户昵称信息。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认10 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回收藏列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.FavoriteResponse](#responsefavoriteresponse) ] } |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /favorites/user/posts

#### GET
##### Summary

获取我收藏的帖子

##### Description

获取当前登录用户收藏的所有帖子，支持分页。后续将完善返回内容，提供作者和分类信息。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认10 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回收藏的帖子列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.PostListResponse](#responsepostlistresponse) ] } |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /favorites/{id}

#### POST
##### Summary

收藏帖子

##### Description

将指定帖子添加到当前登录用户的收藏列表中。如果已经收藏过该帖子，则返回已收藏的提示。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 已收藏该帖子 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 201 | 收藏成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

#### DELETE
##### Summary

取消收藏

##### Description

取消当前登录用户对指定帖子的收藏。如果未收藏过该帖子，则返回相应提示。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 取消收藏成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 404 | 帖子不存在或未收藏过该帖子 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

---
### /likes/check/{id}

#### GET
##### Summary

检查是否已点赞

##### Description

检查当前登录用户是否已经点赞指定帖子。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 检查成功，返回是否已点赞 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.CheckLikeResponse](#responsechecklikeresponse) } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /likes/count/{id}

#### GET
##### Summary

获取帖子点赞数

##### Description

获取指定帖子的点赞数量。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回点赞数量 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.CountResponse](#responsecountresponse) } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

### /likes/post/{id}

#### GET
##### Summary

获取帖子点赞列表

##### Description

获取指定帖子的所有点赞用户，支持分页。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认10 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回点赞列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.LikeResponse](#responselikeresponse) ] } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

### /likes/user

#### GET
##### Summary

获取用户点赞列表

##### Description

获取当前登录用户的所有点赞帖子，支持分页。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认10 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回点赞列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.LikeResponse](#responselikeresponse) ] } |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /likes/{id}

#### POST
##### Summary

点赞帖子

##### Description

对指定帖子进行点赞。如果已经点赞过该帖子，则返回已点赞的提示。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 已点赞该帖子 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 201 | 点赞成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

#### DELETE
##### Summary

取消点赞

##### Description

取消对指定帖子的点赞。如果未点赞过该帖子，则返回相应提示。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 取消点赞成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 404 | 帖子不存在或未点赞过该帖子 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

---
### /notifications

#### GET
##### Summary

获取当前用户的通知列表

##### Description

获取当前登录用户的未读和已读通知，支持分页和状态过滤。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认20 | No | integer |
| status | query | 通知状态 (unread, read, all) | No | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回通知列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.NotificationResponse](#responsenotificationresponse) ] } |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /notifications/read

#### PUT
##### Summary

标记通知为已读

##### Description

将指定ID的通知或所有未读通知标记为已读。只能标记属于当前登录用户的通知。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| request | body | 标记已读请求参数 (notification_ids 或 read_all) | Yes | [request.MarkReadRequest](#requestmarkreadrequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 标记成功，返回剩余未读数 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MarkReadResponse](#responsemarkreadresponse) } |
| 400 | 无效的请求参数 | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 无权标记通知（ID不属于当前用户） | [response.ErrorResponse403](#responseerrorresponse403) |
| 404 | 通知ID不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /notifications/read-all

#### PUT
##### Summary

标记所有通知为已读

##### Description

将当前登录用户的所有通知标记为已读。

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 标记成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /notifications/unread

#### GET
##### Summary

获取未读通知列表

##### Description

获取当前登录用户的所有未读通知，支持分页。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认20 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回未读通知列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.NotificationResponse](#responsenotificationresponse) ] } |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /notifications/unread-count

#### GET
##### Summary

获取未读通知数量

##### Description

获取当前登录用户的未读通知数量。

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 成功返回未读通知数 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.UnreadCountResponse](#responseunreadcountresponse) } |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /notifications/{id}

#### GET
##### Summary

获取通知详情

##### Description

获取指定ID的通知详情。只能获取属于当前登录用户的通知。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 通知ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回通知详细信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.NotificationResponse](#responsenotificationresponse) } |
| 400 | 无效的通知ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 无权查看该通知（不属于当前用户） | [response.ErrorResponse403](#responseerrorresponse403) |
| 404 | 通知不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /notifications/{id}/read

#### PUT
##### Summary

标记通知为已读

##### Description

将指定ID的通知标记为已读。只能标记属于当前登录用户的通知。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 通知ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 标记成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的通知ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 无权标记该通知（不属于当前用户） | [response.ErrorResponse403](#responseerrorresponse403) |
| 404 | 通知不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

---
### /posts

#### GET
##### Summary

获取帖子列表

##### Description

获取所有帖子的列表，支持分页、按分类筛选和按标签筛选。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认10 | No | integer |
| category_id | query | 分类ID，用于筛选特定分类的帖子 | No | integer |
| tag | query | 标签名称，用于筛选包含特定标签的帖子 | No | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回帖子列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.PostListResponse](#responsepostlistresponse) ] } |
| 400 | 无效的请求参数 | [response.ErrorResponse400](#responseerrorresponse400) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

#### POST
##### Summary

创建新帖子

##### Description

创建一个新的帖子，需要提供分类ID、标题、内容和可选的标签列表。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| request | body | 创建帖子请求参数 | Yes | [request.CreatePostRequest](#requestcreatepostrequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 201 | 创建成功，返回帖子详细信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.PostResponse](#responsepostresponse) } |
| 400 | 无效的请求参数，可能是标题太短、内容不足或分类ID无效 | [response.Response](#responseresponse) |
| 401 | 未登录或认证令牌无效 | [response.Response](#responseresponse) |
| 403 | 没有创建帖子的权限 | [response.Response](#responseresponse) |
| 500 | 服务器内部错误 | [response.Response](#responseresponse) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /posts/categories/{id}

#### GET
##### Summary

按分类获取帖子

##### Description

获取特定分类下的所有帖子，支持分页。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 分类ID | Yes | integer |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认10 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回帖子列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.PostListResponse](#responsepostlistresponse) ] } |
| 400 | 无效的分类ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

### /posts/users/{id}

#### GET
##### Summary

获取用户帖子列表

##### Description

获取某个用户发布的所有帖子，支持分页。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 用户ID | Yes | integer |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认10 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回帖子列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.PostListResponse](#responsepostlistresponse) ] } |
| 400 | 无效的用户ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 404 | 用户不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

### /posts/{id}

#### GET
##### Summary

获取帖子详情

##### Description

根据帖子ID获取帖子的详细信息，包括标题、内容、作者、分类、标签等。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回帖子详细信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.PostResponse](#responsepostresponse) } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

#### PUT
##### Summary

更新帖子

##### Description

更新现有帖子的内容，包括标题、内容、分类和标签。只有帖子作者或管理员才能更新帖子。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |
| request | body | 更新帖子请求参数 (title, content, category_id?, tags?) | Yes | [request.UpdatePostRequest](#requestupdatepostrequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 更新成功，返回更新后的帖子详情 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.PostResponse](#responsepostresponse) } |
| 400 | 无效的请求参数或帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 无权修改该帖子（非作者） | [response.ErrorResponse403](#responseerrorresponse403) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

#### DELETE
##### Summary

删除帖子

##### Description

删除指定的帖子。只有帖子作者才能删除帖子。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 删除成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 无权删除该帖子（非作者） | [response.ErrorResponse403](#responseerrorresponse403) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /posts/{id}/status

#### PATCH
##### Summary

更新帖子状态

##### Description

更新帖子的置顶、加精、隐藏和锁定状态。需要管理员权限。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |
| request | body | 更新状态请求参数 | Yes | [request.UpdatePostStatusRequest](#requestupdatepoststatusrequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 更新成功，返回更新后的状态 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.UpdatePostStatusResponse](#responseupdatepoststatusresponse) } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 无权更新帖子状态（非管理员） | [response.ErrorResponse403](#responseerrorresponse403) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

---
### /posts/{id}/favorite

#### POST
##### Summary

收藏帖子

##### Description

收藏指定的帖子。用户必须登录才能收藏帖子。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 收藏成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 409 | 已经收藏过该帖子 | [response.ErrorResponse409](#responseerrorresponse409) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

#### DELETE
##### Summary

取消收藏

##### Description

取消收藏指定的帖子。用户必须登录才能取消收藏。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 取消收藏成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 404 | 帖子不存在或未收藏过该帖子 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /posts/{id}/like

#### POST
##### Summary

点赞帖子

##### Description

对指定帖子进行点赞。用户必须登录才能点赞。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 点赞成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 409 | 已经点赞过该帖子 | [response.ErrorResponse409](#responseerrorresponse409) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

#### DELETE
##### Summary

取消点赞

##### Description

取消对指定帖子的点赞。用户必须登录才能取消点赞。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 取消点赞成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 404 | 帖子不存在或未点赞过该帖子 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

---
### /posts/{id}/tags

#### GET
##### Summary

获取帖子的所有标签

##### Description

获取指定帖子的所有标签。返回标签的基本信息，包括ID、名称和描述。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回标签列表 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [ [response.TagResponse](#responsetagresponse) ] } |
| 400 | 无效的帖子ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 404 | 帖子不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

### /posts/{id}/tags/{tag_id}

#### POST
##### Summary

为帖子添加标签

##### Description

为指定帖子添加一个标签。只有帖子作者或管理员可以为帖子添加标签。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |
| tag_id | path | 标签ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 添加成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的帖子ID或标签ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 没有为帖子添加标签的权限（需要是帖子作者或管理员） | [response.ErrorResponse403](#responseerrorresponse403) |
| 404 | 帖子或标签不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 409 | 帖子已添加该标签 | [response.ErrorResponse409](#responseerrorresponse409) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

#### DELETE
##### Summary

从帖子移除标签

##### Description

从指定帖子移除一个标签。只有帖子作者或管理员可以从帖子移除标签。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 帖子ID | Yes | integer |
| tag_id | path | 标签ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 移除成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的帖子ID或标签ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 没有从帖子移除标签的权限（需要是帖子作者或管理员） | [response.ErrorResponse403](#responseerrorresponse403) |
| 404 | 帖子或标签不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 409 | 帖子未添加该标签 | [response.ErrorResponse409](#responseerrorresponse409) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /tags

#### GET
##### Summary

获取标签列表

##### Description

获取所有标签的列表，支持分页。返回标签的基本信息，包括ID、名称、描述和使用该标签的帖子数量。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认50 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回标签列表和分页信息 | [response.PaginatedResponse](#responsepaginatedresponse) & { **"data"**: [ [response.TagResponse](#responsetagresponse) ] } |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

#### POST
##### Summary

创建标签

##### Description

创建一个新的标签，需要提供标签名称和可选的描述。只有管理员可以创建标签。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| request | body | 创建标签请求参数 | Yes | [request.CreateTagRequest](#requestcreatetagrequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 201 | 创建成功，返回新创建的标签信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.TagResponse](#responsetagresponse) } |
| 400 | 无效的请求参数，可能是标签名称太短或格式不正确 | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 没有创建标签的权限（需要管理员权限） | [response.ErrorResponse403](#responseerrorresponse403) |
| 409 | 标签名称已存在 | [response.ErrorResponse409](#responseerrorresponse409) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /tags/popular

#### GET
##### Summary

获取热门标签

##### Description

获取使用最多的热门标签，按照使用该标签的帖子数量降序排序。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| limit | query | 数量限制，默认10 | No | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回热门标签列表 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [ [response.TagResponse](#responsetagresponse) ] } |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

### /tags/{id}

#### GET
##### Summary

获取标签详情

##### Description

根据标签ID获取标签的详细信息，包括名称、描述和使用该标签的帖子数量。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 标签ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回标签详细信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.TagResponse](#responsetagresponse) } |
| 400 | 无效的标签ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 404 | 标签不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

#### PUT
##### Summary

更新标签

##### Description

更新现有标签的信息，包括名称和描述。只有管理员可以更新标签。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 标签ID | Yes | integer |
| request | body | 更新标签请求参数 | Yes | [request.UpdateTagRequest](#requestupdatetagrequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 更新成功，返回更新后的标签信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.TagResponse](#responsetagresponse) } |
| 400 | 无效的请求参数或标签ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 没有更新标签的权限（需要管理员权限） | [response.ErrorResponse403](#responseerrorresponse403) |
| 404 | 标签不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 409 | 标签名称已存在 | [response.ErrorResponse409](#responseerrorresponse409) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

#### DELETE
##### Summary

删除标签

##### Description

删除指定的标签。只有管理员可以删除标签。删除标签会导致使用该标签的帖子失去该标签。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 标签ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 删除成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的标签ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未登录或认证令牌无效 | [response.ErrorResponse401](#responseerrorresponse401) |
| 403 | 没有删除标签的权限（需要管理员权限） | [response.ErrorResponse403](#responseerrorresponse403) |
| 404 | 标签不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

---
### /search

#### GET
##### Summary

全局搜索

##### Description

在全站范围内搜索帖子和用户。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| q | query | 搜索关键词 | Yes | string |
| type | query | 搜索类型 (posts, users, all) | No | string |
| page | query | 页码，默认1 | No | integer |
| page_size | query | 每页数量，默认10 | No | integer |
| sort_by | query | 排序字段 (relevance, time) | No | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 搜索成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.SearchResponse](#responsesearchresponse) } |
| 400 | example({\\"缺少搜索关键词 'q'\\"}) | [response.ErrorResponse400](#responseerrorresponse400) |
| 500 | example({\\"服务器内部错误\\"}) | [response.ErrorResponse500](#responseerrorresponse500) |

---
### /stats

#### GET
##### Summary

获取论坛统计数据

##### Description

获取论坛的用户数、帖子数、评论数、分类数和标签数等统计数据

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回统计数据 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [stats.Stats](#statsstats) } |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

---
### /upload

#### POST
##### Summary

上传文件

##### Description

通用文件上传接口，用于上传头像、帖子图片等。需要认证。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| file | formData | 要上传的文件 | Yes | file |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 201 | 文件上传成功，返回文件URL | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.UploadResponse](#responseuploadresponse) } |
| 400 | 无效的请求 (没有文件、文件过大、格式不支持) | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 用户未认证 | [response.ErrorResponse401](#responseerrorresponse401) |
| 500 | 服务器内部错误 (文件保存失败) | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

---
### /users/avatar

#### PUT
##### Summary

更新当前用户头像

##### Description

允许当前登录用户上传并更新自己的头像，需要提供文件上传后获取的头像URL。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| request | body | 更新头像请求参数 (avatar_url) | Yes | [request.UpdateAvatarRequest](#requestupdateavatarrequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 更新成功，返回新的头像URL | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.UpdateAvatarResponse](#responseupdateavatarresponse) } |
| 400 | 无效的请求参数或URL | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 用户未认证 | [response.ErrorResponse401](#responseerrorresponse401) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /users/login

#### POST
##### Summary

用户登录

##### Description

用户通过用户名和密码登录系统，成功后返回JWT令牌用于后续认证。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| request | body | 登录请求参数 | Yes | [request.LoginRequest](#requestloginrequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 登录成功，返回用户信息和JWT令牌 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.LoginResponse](#responseloginresponse) } |
| 400 | 无效的请求参数 | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 用户不存在或密码错误 | [response.ErrorResponse401](#responseerrorresponse401) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

### /users/password

#### PUT
##### Summary

修改用户密码

##### Description

修改当前登录用户的密码，需要提供原密码、新密码和确认密码。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| request | body | 修改密码请求参数 | Yes | [request.ChangePasswordRequest](#requestchangepasswordrequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 密码修改成功 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.MessageResponse](#responsemessageresponse) } |
| 400 | 无效的请求参数或原密码错误 | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 用户未认证 | [response.ErrorResponse401](#responseerrorresponse401) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /users/profile

#### PUT
##### Summary

更新用户个人资料

##### Description

更新当前登录用户的个人资料信息，包括昵称和个人简介。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| request | body | 更新个人资料请求参数 (nickname, bio) | Yes | [request.UpdateProfileRequest](#requestupdateprofilerequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 更新成功，返回更新后的完整用户信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.UserProfileResponse](#responseuserprofileresponse) } |
| 400 | 无效的请求参数 | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 用户未认证 | [response.ErrorResponse401](#responseerrorresponse401) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /users/refresh-token

#### POST
##### Summary

刷新JWT令牌

##### Description

使用当前有效的JWT令牌获取新的令牌，延长用户会话有效期。

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 刷新成功，返回新的JWT令牌 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.TokenResponse](#responsetokenresponse) } |
| 400 | 认证格式无效 | [response.ErrorResponse400](#responseerrorresponse400) |
| 401 | 未提供认证令牌或无法刷新令牌 | [response.ErrorResponse401](#responseerrorresponse401) |

##### Security

| Security Schema | Scopes |
| --------------- | ------ |
| BearerAuth |  |

### /users/register

#### POST
##### Summary

创建新用户

##### Description

创建一个新的用户，并返回创建的用户信息和JWT令牌。用户需要提供用户名、密码、电子邮件和昵称。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| request | body | 注册请求参数 | Yes | [request.RegisterRequest](#requestregisterrequest) |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 201 | 注册成功，返回用户ID和JWT令牌 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.LoginResponse](#responseloginresponse) } |
| 400 | 无效的请求参数或用户名已存在 | [response.ErrorResponse400](#responseerrorresponse400) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

### /users/{id}

#### GET
##### Summary

获取指定ID的用户信息

##### Description

根据用户的ID获取详细信息，包括用户名、昵称、头像等公开资料。

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ------ |
| id | path | 用户ID | Yes | integer |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | 获取成功，返回用户详细信息 | [response.ResponseWithData](#responseresponsewithdata) & { **"data"**: [response.UserResponse](#responseuserresponse) } |
| 400 | 无效的用户ID | [response.ErrorResponse400](#responseerrorresponse400) |
| 404 | 用户不存在 | [response.ErrorResponse404](#responseerrorresponse404) |
| 500 | 服务器内部错误 | [response.ErrorResponse500](#responseerrorresponse500) |

---
### Models

#### request.ChangePasswordRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| new_password | string | *Example:* `"newpassword456"` | Yes |
| old_password | string | *Example:* `"oldpassword123"` | Yes |

#### request.CreateCategoryRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| description | string | *Example:* `"关于技术的讨论区"` | No |
| name | string | *Example:* `"技术讨论"` | Yes |
| parent_id | integer | *Example:* `0` | No |
| slug | string | *Example:* `"tech-discussion"` | Yes |

#### request.CreateCommentRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| content | string | 评论内容<br>*Example:* `"这是评论内容"` | Yes |
| parent_id | integer | 父评论ID (可选, 用于回复)<br>*Example:* `10` | No |
| post_id | integer | 帖子ID<br>*Example:* `1` | Yes |

#### request.CreatePostRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| category_id | integer | *Example:* `1` | Yes |
| content | string | *Example:* `"这是我学习Go语言的一些心得体会..."` | Yes |
| tag_ids | [ integer ] | *Example:* `[1]` | No |
| tags | [ string ] | *Example:* `["Go"]` | No |
| title | string | *Example:* `"Go语言学习心得"` | Yes |

#### request.CreateTagRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| description | string | *Example:* `"Go语言相关内容"` | No |
| name | string | *Example:* `"Go"` | Yes |

#### request.LoginRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| password | string | *Example:* `"password123"` | Yes |
| username | string | 可以是用户名或邮箱<br>*Example:* `"johndoe"` | Yes |

#### request.MarkReadRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| notification_ids | [ integer ] | *Example:* `[1,2,3]` | No |
| read_all | boolean | *Example:* `false` | No |

#### request.RegisterRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| email | string | *Example:* `"john@example.com"` | Yes |
| nickname | string | *Example:* `"John Doe"` | Yes |
| password | string | *Example:* `"password123"` | Yes |
| username | string | *Example:* `"johndoe"` | Yes |

#### request.ReplyCommentRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| content | string | 回复内容<br>*Example:* `"这是回复内容"` | Yes |

#### request.UpdateAvatarRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| avatar_url | string | *Example:* `"https://example.com/avatar.jpg"` | Yes |

#### request.UpdateCategoryRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| description | string | *Example:* `"更新后的分类描述"` | No |
| name | string | *Example:* `"更新后的分类名称"` | No |

#### request.UpdateCommentRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| content | string | 更新后的评论内容<br>*Example:* `"更新后的评论内容"` | Yes |

#### request.UpdatePostRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| category_id | integer | *Example:* `2` | No |
| content | string | *Example:* `"更新后的帖子内容..."` | No |
| tag_ids | [ integer ] | *Example:* `[2]` | No |
| title | string | *Example:* `"更新后的帖子标题"` | No |

#### request.UpdatePostStatusRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| is_featured | boolean | *Example:* `true` | No |
| is_hidden | boolean | *Example:* `true` | No |
| is_locked | boolean | *Example:* `true` | No |
| is_pinned | boolean | *Example:* `true` | No |

#### request.UpdateProfileRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| avatar | string | *Example:* `"https://example.com/avatar.jpg"` | No |
| bio | string | *Example:* `"我是一名软件开发者"` | No |
| nickname | string | *Example:* `"John Smith"` | No |

#### request.UpdateTagRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| description | string | *Example:* `"Go语言编程相关内容"` | No |
| name | string | *Example:* `"Golang"` | Yes |

#### request.UpdateUserRoleRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| role | string | *Example:* `"admin"` | Yes |

#### request.UpdateUserStatusRequest

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| status | string | *Example:* `"active"` | Yes |

#### response.CategoryInfo

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| id | integer | *Example:* `1` | No |
| name | string | *Example:* `"技术分享"` | No |
| parent_id | integer | 使用数字而非字符串"null"<br>*Example:* `0` | No |

#### response.CategoryResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| children | [ [response.CategoryResponse](#responsecategoryresponse) ] | Optional: For nested categories | No |
| created_at | string | *Example:* `"2023-10-26T08:00:00Z"` | No |
| description | string | *Example:* `"关于技术的讨论"` | No |
| id | integer | *Example:* `1` | No |
| name | string | *Example:* `"技术分享"` | No |
| parent_id | integer | 使用数字而非字符串"null"<br>*Example:* `0` | No |
| post_count | integer | Optional: Count of posts<br>*Example:* `50` | No |
| updated_at | string | *Example:* `"2023-10-26T09:00:00Z"` | No |

#### response.CheckFavoriteResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| is_favorited | boolean | *Example:* `true` | No |

#### response.CheckLikeResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| is_liked | boolean | *Example:* `true` | No |

#### response.CommentListItem

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| author | [response.UserInfo](#responseuserinfo) | 评论作者信息 | No |
| content | string | *Example:* `"User's comment content."` | No |
| created_at | string | *Example:* `"2023-10-27T11:05:00Z"` | No |
| id | integer | *Example:* `105` | No |
| post_id | integer | *Example:* `10` | No |
| post_title | string | 关联的帖子标题<br>*Example:* `"Related Post Title"` | No |

#### response.CommentResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| author | [response.UserInfo](#responseuserinfo) | Use UserInfo | No |
| content | string | *Example:* `"Updated comment content."` | No |
| created_at | string | *Example:* `"2023-10-27T11:00:00Z"` | No |
| id | integer | *Example:* `101` | No |
| likes | integer | *Example:* `5` | No |
| parent_id | integer | *Example:* `99` | No |
| post_id | integer | *Example:* `1` | No |
| replies | [ [response.CommentResponse](#responsecommentresponse) ] | 嵌套回复 | No |
| updated_at | string | *Example:* `"2023-10-27T12:00:00Z"` | No |

#### response.CountResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| count | integer | *Example:* `128` | No |

#### response.ErrorResponse400

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| code | integer | *Example:* `400` | No |
| message | string | *Example:* `"无效的请求参数"` | No |

#### response.ErrorResponse401

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| code | integer | *Example:* `401` | No |
| message | string | *Example:* `"未登录或认证令牌无效"` | No |

#### response.ErrorResponse403

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| code | integer | *Example:* `403` | No |
| message | string | *Example:* `"无权访问该资源"` | No |

#### response.ErrorResponse404

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| code | integer | *Example:* `404` | No |
| message | string | *Example:* `"资源不存在"` | No |

#### response.ErrorResponse409

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| code | integer | *Example:* `409` | No |
| message | string | *Example:* `"资源冲突"` | No |

#### response.ErrorResponse500

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| code | integer | *Example:* `500` | No |
| message | string | *Example:* `"服务器内部错误"` | No |

#### response.FavoriteResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| created_at | string | *Example:* `"2023-10-27T14:00:00Z"` | No |
| id | integer | *Example:* `1` | No |
| nickname | string | Optional: Requires joining/fetching<br>*Example:* `"John Doe"` | No |
| post_id | integer | *Example:* `10` | No |
| post_title | string | Optional: Requires joining/fetching<br>*Example:* `"帖子标题"` | No |
| user_id | integer | *Example:* `1` | No |

#### response.LikeResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| created_at | string | *Example:* `"2023-10-27T15:00:00Z"` | No |
| id | integer | *Example:* `1` | No |
| nickname | string | Optional: Requires joining/fetching<br>*Example:* `"Jane Doe"` | No |
| post_id | integer | *Example:* `10` | No |
| user_id | integer | *Example:* `1` | No |

#### response.LoginResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| expires_at | integer |  | No |
| nickname | string |  | No |
| role | string |  | No |
| token | string |  | No |
| user_id | integer |  | No |
| username | string |  | No |

#### response.MarkReadResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| message | string | 提示信息<br>*Example:* `"Notifications marked as read."` | No |
| unread_count | integer | 剩余未读通知数量<br>*Example:* `2` | No |

#### response.MessageResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| message | string | 提示信息<br>*Example:* `"操作成功"` | No |

#### response.NotificationResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| content | string | 通知内容简述<br>*Example:* `"John Doe 回复了您的评论: '说得好！'"` | No |
| created_at | string | 通知创建时间<br>*Example:* `"2023-10-27T10:00:00Z"` | No |
| id | integer | *Example:* `1` | No |
| is_read | boolean | 是否已读<br>*Example:* `false` | No |
| related_comment_id | integer | 关联评论ID (如果适用) | No |
| related_post | [response.PostInfo](#responsepostinfo) | 关联帖子 (如果适用) | No |
| sender | [response.UserInfo](#responseuserinfo) | 发送者 (如果适用，例如系统通知可能没有发送者) | No |
| title | string | 通知标题 (可选)<br>*Example:* `"您收到了新的回复"` | No |
| type | string | 通知类型: system, comment_reply, post_like, comment_like, new_follower ...<br>*Example:* `"comment_reply"` | No |

#### response.PaginatedResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| data |  | 当前页数据列表 | No |
| page | integer | 当前页码<br>*Example:* `1` | No |
| page_size | integer | 每页数量<br>*Example:* `10` | No |
| total | integer | 总记录数<br>*Example:* `150` | No |

#### response.PostInfo

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| id | integer | *Example:* `10` | No |
| title | string | *Example:* `"关于论坛的建议"` | No |

#### response.PostListResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| author | [response.UserInfo](#responseuserinfo) | 作者信息 | No |
| category | [response.CategoryInfo](#responsecategoryinfo) | 分类信息 | No |
| comment_count | integer | 评论数<br>*Example:* `32` | No |
| created_at | string | 创建时间<br>*Example:* `"2023-10-26T09:00:00Z"` | No |
| has_attachment | boolean | 是否包含附件<br>*Example:* `true` | No |
| id | integer | 帖子ID<br>*Example:* `1` | No |
| is_favorited | boolean | 当前用户是否已收藏<br>*Example:* `false` | No |
| is_featured | boolean | 是否精华<br>*Example:* `false` | No |
| is_hidden | boolean | 是否隐藏<br>*Example:* `false` | No |
| is_liked | boolean | 当前用户是否已点赞<br>*Example:* `false` | No |
| is_locked | boolean | 是否锁定<br>*Example:* `false` | No |
| is_pinned | boolean | 是否置顶<br>*Example:* `false` | No |
| last_reply_at | string | 最后回复时间（可选）<br>*Example:* `"2023-10-26T11:00:00Z"` | No |
| like_count | integer | 点赞数<br>*Example:* `128` | No |
| summary | string | 帖子内容摘要（可选）<br>*Example:* `"内容摘要..."` | No |
| tags | [ [response.TagInfo](#responsetaginfo) ] | 标签列表 | No |
| title | string | 帖子标题<br>*Example:* `"欢迎来到论坛"` | No |
| view_count | integer | 浏览次数<br>*Example:* `1024` | No |

#### response.PostResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| attachment_count | integer | 附件数量（可选）<br>*Example:* `2` | No |
| author | [response.UserInfo](#responseuserinfo) | 作者信息 | No |
| category | [response.CategoryInfo](#responsecategoryinfo) | 分类信息 | No |
| comment_count | integer | 评论数<br>*Example:* `32` | No |
| content | string | 帖子内容<br>*Example:* `"这是帖子的内容..."` | No |
| created_at | string | 创建时间<br>*Example:* `"2023-10-26T09:00:00Z"` | No |
| id | integer | 帖子ID<br>*Example:* `1` | No |
| is_favorited | boolean | 当前用户是否已收藏<br>*Example:* `false` | No |
| is_featured | boolean | 是否精华（管理员字段）<br>*Example:* `false` | No |
| is_hidden | boolean | 是否隐藏（管理员字段）<br>*Example:* `false` | No |
| is_liked | boolean | 当前用户是否已点赞<br>*Example:* `false` | No |
| is_locked | boolean | 是否锁定（禁止回复）<br>*Example:* `false` | No |
| is_pinned | boolean | 是否置顶（管理员字段）<br>*Example:* `false` | No |
| last_reply_at | string | 最后回复时间（可选）<br>*Example:* `"2023-10-26T11:00:00Z"` | No |
| like_count | integer | 点赞数<br>*Example:* `128` | No |
| summary | string | 帖子内容摘要（可选）<br>*Example:* `"内容摘要..."` | No |
| tags | [ [response.TagInfo](#responsetaginfo) ] | 标签列表 | No |
| title | string | 帖子标题<br>*Example:* `"欢迎来到论坛"` | No |
| updated_at | string | 更新时间<br>*Example:* `"2023-10-26T10:00:00Z"` | No |
| view_count | integer | 浏览次数<br>*Example:* `1024` | No |

#### response.Response

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| code | integer |  | No |
| data |  |  | No |
| message | string |  | No |

#### response.ResponseWithData

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| code | integer | *Example:* `200` | No |
| data |  |  | No |
| message | string | *Example:* `"成功"` | No |

#### response.SearchResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| query | string | 搜索关键词<br>*Example:* `"search term"` | No |
| results | [response.SearchResult](#responsesearchresult) | 搜索结果 | No |
| type | string | 搜索类型 ('posts', 'users', 'all')<br>*Example:* `"all"` | No |

#### response.SearchResult

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| posts | [response.PaginatedResponse](#responsepaginatedresponse) |  | No |
| users | [response.PaginatedResponse](#responsepaginatedresponse) |  | No |

#### response.TagInfo

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| id | integer | *Example:* `1` | No |
| name | string | *Example:* `"Go"` | No |

#### response.TagResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| created_at | string | *Example:* `"2023-10-26T08:30:00Z"` | No |
| id | integer | *Example:* `1` | No |
| name | string | *Example:* `"Golang"` | No |
| post_count | integer | Optional: Count of posts with this tag<br>*Example:* `100` | No |
| updated_at | string | *Example:* `"2023-10-26T08:30:00Z"` | No |

#### response.TokenResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| expires_at | integer | *Example:* `1714913563` | No |
| token | string | *Example:* `"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."` | No |

#### response.UnreadCountResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| unread_count | integer | 未读通知数量<br>*Example:* `5` | No |

#### response.UpdateAvatarResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| avatar_url | string | *Example:* `"/path/to/new_avatar.jpg"` | No |
| message | string | *Example:* `"Avatar updated successfully."` | No |

#### response.UpdatePostStatusResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| is_featured | boolean | *Example:* `true` | No |
| is_hidden | boolean | *Example:* `false` | No |
| is_locked | boolean | *Example:* `false` | No |
| is_pinned | boolean | *Example:* `true` | No |
| post_id | integer | *Example:* `1` | No |

#### response.UpdateUserRoleResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| message | string | *Example:* `"User role updated successfully."` | No |
| new_role | string | *Example:* `"moderator"` | No |
| user_id | integer | *Example:* `1` | No |

#### response.UpdateUserStatusResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| message | string | *Example:* `"User status updated successfully."` | No |
| new_status | string | *Example:* `"inactive"` | No |
| user_id | integer | *Example:* `1` | No |

#### response.UploadResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| file_name | string | *Example:* `"avatar.jpg"` | No |
| file_size | integer | *Example:* `12345` | No |
| file_url | string | *Example:* `"http://localhost:8080/uploads/images/avatar.jpg"` | No |
| message | string | *Example:* `"文件上传成功"` | No |

#### response.UserInfo

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| avatar | string | *Example:* `"/path/to/avatar.jpg"` | No |
| id | integer | *Example:* `5` | No |
| nickname | string | *Example:* `"John Doe"` | No |

#### response.UserListInfo

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| avatar | string |  | No |
| created_at | string |  | No |
| email | string |  | No |
| id | integer |  | No |
| last_login | string | 注意：domain.User 可能没有 LastLogin 字段，需要确认 | No |
| nickname | string |  | No |
| role | string |  | No |
| status | string |  | No |
| username | string |  | No |

#### response.UserProfileResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| avatar | string |  | No |
| bio | string | Added Bio | No |
| created_at | string |  | No |
| email | string |  | No |
| id | integer |  | No |
| nickname | string |  | No |
| role | string | Added Role | No |
| status | string | Added Status | No |
| updated_at | string |  | No |
| username | string |  | No |

#### response.UserResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| avatar | string | *Example:* `"/path/to/avatar.jpg"` | No |
| bio | string | *Example:* `"A short bio here."` | No |
| created_at | string | *Example:* `"2023-10-26T08:00:00Z"` | No |
| id | integer | *Example:* `2` | No |
| nickname | string | *Example:* `"John Doe"` | No |
| stats | [response.UserStats](#responseuserstats) |  | No |

#### response.UserStats

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| comment_count | integer |  | No |
| follower_count | integer |  | No |
| following_count | integer |  | No |
| post_count | integer |  | No |

#### stats.Stats

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| category_count | integer |  | No |
| comment_count | integer |  | No |
| post_count | integer |  | No |
| tag_count | integer |  | No |
| user_count | integer |  | No |

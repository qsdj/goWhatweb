# goWhatweb
[go语言写的web指纹识别] Identify websites by go language
## Useage

## Feature&&Checklist
1. [ ] 内存加载指纹,在内存中对每个指纹的命中率进行统计，优先使用命中率高的指纹。（就是对排序数组中的路径hash做一次位置替换）

2. 根据统计，指纹可能达到了1800+，累计访问路径会达到1600,如果访问一个不知名的cms至少会请求1600+次，减少请求&提高效率的方法：
    1. [ ] 判断网站连通性，在请求之前，会访问一次首页，存储时间间隔来用于下一部判断，若时间间隔较长，则取消该次请求，标记该URL失败原因为“请求首页时间间隔过长”
    2. [ ] 优先使用命中率高的指纹，其次是排序(同一个访问地址有不同的cms指纹情况)中较高的指纹，在识别成功后立即关闭其他请求。
    3. [ ] 优先使用head请求，head为200才会进行下一步get请求。
    4. [ ] 每发包200次会随机延时片刻，在请求首页是否正常，若不正常则延时更大片刻，延时超过1分钟则取消该URL其他请求，标记该URL失败原因为“请求量过大 {}次请求后网页无法访问”
    5. [ ] 将请求UA伪装为随机爬虫
3. 上述针对单个URL的指纹识别可能无法发挥机器的最大效率，所以设计上goWhatweb对多个URL同时请求指纹分析，并执行上述规则，每个URL的指纹分析是独立的，互不干扰的。所以，更多的URL会将机器的性能和宽带性能发挥到最大。
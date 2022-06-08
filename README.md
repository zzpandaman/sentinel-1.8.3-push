- 这个仓库主要记录如何将源码各种规则的持久化改造成push模式，步骤如下。
  - 第一次提交是解压的源码，第二次提交覆盖源码中的readme；后续仅修改sentinel-dashboard模块。
  - 验证FlowControllerV2中配置flowRuleNacosProvider、flowRuleNacosPublisher可用
    - 把sentinel-dashboard/src/test/java/com/alibaba/csp/sentinel/dashboard/rule/nacos文件夹拷到<br>
    sentinel-dashboard/src/main/java/com/alibaba/csp/sentinel/dashboard/rule
    - 修改其下NacosConfig做相应配置
    - 修改FlowControllerV2使用flowRuleNacosProvider、flowRuleNacosPublisher，即Nacos流控规则
    - 修改前端页面，添加调用FlowControllerV2的页签<br>
    sentinel-dashboard/src/main/webapp/resources/app/scripts/directives/sidebar/sidebar.html
  - 参考FlowControllerV2修改FlowControllerV1
    - 为什么选择改V1:簇点链路页面、流控规则页面中添加规则默认使用V1，这样不用改前端调用就能全部走新的规则。
    - 修改点就是添加flowRuleNacosProvider、flowRuleNacosPublisher，并在相应位置使用它们。
- 修改其他规则
  - 添加相应规则的DynamicRulePublisher 和 DynamicRuleProvider接口实现。<br>
  [官方文档](https://github.com/alibaba/Sentinel/wiki/%E5%9C%A8%E7%94%9F%E4%BA%A7%E7%8E%AF%E5%A2%83%E4%B8%AD%E4%BD%BF%E7%94%A8-Sentinel)对两个接口的说明是:
    > 从 Sentinel 1.4.0 开始，我们抽取出了接口用于向远程配置中心推送规则以及拉取规则：
    > - DynamicRuleProvider<T>: 拉取规则
    > - DynamicRulePublisher<T>: 推送规则

# SCO 首页配置生成

## 一、页面注解

依赖： 
```sh
  kapt project(':SCOCompiler')
  api project(':SCOAnnotation')
```

注解 @SCOConfig例：
```sh
@SCOConfig(
        title = "第二个页面",//页面对应 模块名称
        businessType = SCOConstantConfigs.Business_AfterSale,//页面对应模块的 业务类型
        permission = "第二个页面权限",//页面对应 模块权限
        moduleSystem = BusinessSystemType.COMMON,//模块对应 系统类别
        moduleShowImgSource = "http://xxxxxImage.jpg",//页面对应模块在首页呈现的icon
        group = SCOConstantConfigs.module_class_sso,//当前页面 所在module包的给定generate class后缀名
        routerPath = "/test/main2",//当前页面跳转路由
        parentRouterPath = "",//当前页面的父级页面路由 如果已经是顶级页面 默认root
        isCanDelete = false,//默认 true ，当前页面对应的模块在首页操作中是否能增删
        isCanMove = false//默认 true ，当前页面对应的模块在首页操作中是否能移动
)
```
### 1.title 页面对应模块的名称

### 2.businessType 页面对应模块的 业务类型

对应注解字段 businessType
如果某个模块属于 不止一个业务类型 则用 | 做分割线拼接字符串 例如
```sh
SCOConstantConfigs.Business_Sale + "|" + SCOConstantConfigs.Business_AfterSale
```

| 业务类型 | 代码 | 描述 |
| -------- | ---- | ---- |
| 销售  | SCOConstantConfigs.Business_Sale |  |
| 售后  | SCOConstantConfigs.Business_AfterSale |  |
| 行政  | SCOConstantConfigs.Business_Administrative |  |

### 3.permission 页面对应模块的 所属权限

### 4.moduleSystem 功能模块系统分类枚举

对应注解字段 moduleSystem

| 所属系统 | 代码 | 描述 |
| -------- | ---- | ---- |
| COMMON  | BusinessSystemType.COMMON | 无需权限，所有人都能用的模块 |
| SSO  | BusinessSystemType.SSO | SSO权限，暂时没有模块 |
| ERP  | BusinessSystemType.ERP | ERP权限 |
| CRM  | BusinessSystemType.CRM | CRM权限 |
| OA  | BusinessSystemType.OA | OA权限 |
| NC  | BusinessSystemType.NC | NC权限 |

### 5.moduleShowImgSource 页面对应模块在首页展示的icon名字 例如：aaa.png 的 aaa

### 6.group 各个单独module生成的class后缀名

对应注解字段 group 

| 所属module | 代码 | 描述 |
| -------- | ---- | ---- |
| app  | SCOConstantConfigs.module_class_app |  |
| core  | SCOConstantConfigs.module_class_core |  |
| sso  | SCOConstantConfigs.module_class_sso |  |
| erp  | SCOConstantConfigs.module_class_erp |  |
| crm  | SCOConstantConfigs.module_class_crm |  |
| oa  | SCOConstantConfigs.module_class_oa |  |
| nc  | SCOConstantConfigs.module_class_nc |  |

### 7.routerPath 当前页面的 router

### 8.parentRouterPath 当前页面的父级页面的 router，默认是root 如没有父级页面 可以不写

### 9.isCanDelete 默认 true ，当前页面对应的模块在首页操作中是否能增删

### 10.isCanMove 默认 true ，当前页面对应的模块在首页操作中是否能移动

## 二、api调用

依赖： 
```sh
	api project(':SCOApi')
```
###  SCOAptApiHelper 获取数据
```sh
    List<ScoModuleMeta> tempListAll = SCOAptApiHelper.getInstance().getTempListAll(this);

    List<ScoModuleMeta> allModuleConfigsBySystemType = SCOAptApiHelper.getInstance().getAllModuleConfigsBySystemType(this, BusinessSystemType.OA);

    Set<String> blackNames = new HashSet<>();
        blackNames.add("第二个页面");
        blackNames.add("Main3Activity");
        blackNames.add("Main4Activity");
        blackNames.add("Main5Activity");
        blackNames.add("Main6Activity");
   
    List<ScoModuleMeta> allModuleConfigsBySystemType1 = SCOAptApiHelper.getInstance().getAllModuleConfigsBySystemType(this, BusinessSystemType.OA, blackNames);
```
* getTempListAll(Context mC) :获取所有模块list

* getTempMapForTitle(Context mC) :获取以模块 title 为key 的 map

* getTempMapForRoute(Context mC) :获取以模块 route 为key 的 map

* getOneByTitle(Context context, String title) :获取当前title 对应的模块

* getAllModuleConfigsByBusinessType(Context context, String BusinessType) :获取对应  业务 类型的全部 list

* getAllModuleConfigsByBusinessType(Context context, String BusinessType, Set<String> blackSet)  :获取对应  业务 类型的剔除掉黑名单后的 list

* getAllModuleConfigsBySystemType(Context context, BusinessSystemType type) :获取对应  系统 类型的全部 list

* getAllModuleConfigsBySystemType(Context context, BusinessSystemType type, Set<String> blackSet) :获取对应  系统 类型的剔除掉黑名单后的 list


###  SCOAptApiUtil 获取数据

* SCOAptApiUtil.getResourceByName(图片名称,Context) :图片名称获取图片的资源id的方法

* SCOAptApiUtil.getResourceByReflect(图片名称,默认图片id) :图片名称获取图片的资源id的方法

* SCOAptApiUtil.getAllModuleConfigs(Context) :获取所有模块配置

* SCOAptApiUtil.getAllModuleConfigsBySystemType(Context,BusinessSystemType) :通过BusinessSystemType 获取配置

* SCOAptApiUtil.getAllModuleConfigsByBusinessType(Context,businessType) :通过 businessType 获取特定的 配置(销售，售后，行政)

* SCOAptApiUtil.getAllModuleConfigsByTitle(Context,title) :通过 title  获取特定的 一条配置

* SCOAptApiUtil.getAllModuleConfigsByRouterPath(Context,title) :通过 routerPath 获取特定的 一条配置

* SCOAptApiUtil.getAllModuleConfigs(Context) :获取所有模块配置list

* SCOAptApiUtil.getConfigMapForRouter(Context) :获取 以 router 为key 的map配置

* SCOAptApiUtil.getConfigMapForTitle(Context) :获取 以 title 为key 的map配置

* SCOAptApiUtil.getConfigSaleList(Context) :获取 sale config list

* SCOAptApiUtil.getConfigAfterSaleList(Context) :获取 after sale config list

* SCOAptApiUtil.getConfigAdministrativeList(Context) :获取 administrative config list

* SCOAptApiUtil.getConfigMap(Context) :获取对应方法的 配置 map

* SCOAptApiUtil.getConfigList(Context) :获取对应方法的 配置 list

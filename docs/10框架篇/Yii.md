- 工作中或多或少会用到的关于 Yii2 的小技巧的一个总结，包括model、controller、view或者配置文件的一些写法。

### 模型相关

#### 获取查询SQL

```php
$query = User::find()->where(['LIKE', 'name', 'ad%', false]);

$commandQuery = clone $query;
echo $commandQuery->createCommand()->getRawSql(); // SELECT * FROM `user` WHERE `name` LIKE 'ad%'
```

#### 查询数据表中具体列

```php
return \yii\helpers\ArrayHelper::getColumn(User::find()->all(), 'name');

return User::find()->select('name')->asArray()->column();
```

#### `Expression()`函数

- 使用`find_in_set()`等函数，需要用到`Expression()` 表达式。

```php
User::find()
    ->where(new yii\db\Expression('FIND_IN_SET(:status, status)'))
    ->addParams([':status' => 1])
    ->all();
```

- 避免select里的子查询被识别成字段

```php
$quert = User::find()
	->select([
          new Expression('count(*) as count , count(distinct mobile) as mnumber')
     ])->asArray()
    ->all();
```

#### 模型中事务编写

```php
Yii::$app->db->transaction(function() {
    $order = new Order($customer);
    $order->save();
    $order->addItems($items);
});

// 这相当于下列冗长的代码：
$transaction = Yii::$app->db->beginTransaction();
try {
    $order = new Order($customer);
    $order->save();
    $order->addItems($items);
    $transaction->commit();
} catch (\Exception $e) {
    $transaction->rollBack();
    throw $e;
}
```

#### Model 里 rules 联合唯一规则

即建立了联合唯一索引的字段，验证时保证数据的完整。

```php
[['store_id', 'member_name'], 'unique', 'targetAttribute' => ['store_id', 'member_name'], 'message' => 'The combination of Store ID and Member Name has already been taken.'],
```

#### 是否存在的规则

校验 `country_id` 在 `Country` 中是否存在，一般用于一些外键关联的数据表之间的数据约束。

```php
public function rules()
{
	return [
    	[['country_id'], 'exist', 'skipOnError' => true, 'targetClass' => Country::className(), 'targetAttribute' => ['country_id' => 'id'],'message' => '此{attribute}不存在。'],
    ];
}
```

#### 表单验证两个字段选取一个

```php
public function rules()
{
    return [
        [['card_id', 'card_code'], function ($attribute, $param) { //至少要一个
            if (empty($this->card_code) && empty($this->card_id)) {
                $this->addError($attribute, 'card_id/card_code至少要填一个');
            }
        }, 'skipOnEmpty' => false],
    ];
}
```

#### Like 模糊查询

```php
$query = User::find()->where(['LIKE', 'name', 'ad%', false]); // SELECT * FROM `user` WHERE `name` LIKE 'ad%'
```

#### 执行SQL查询并缓存结果

调用`yii\db\Connection`的cache方法，写入回调函数执行SQL查询并缓存结果。

```php
$id = Yii::$app->request->get('id');
$collection = Yii::$app->db->cache(function (Connection $db) use($id){
	return self::findOne(['id'=>$id]);
},10); // 缓存10秒
var_dump($collection);
```

#### `and`和`or`共用

在查询时，`and`和`or`条件共用。

```php
Topic::updateAll(
    ['last_comment_time' => new Expression('created_at')],
//  ['or', ['type' => Topic::TYPE, 'last_comment_username' => ''], ['type' => Topic::TYPE, 'last_comment_username' => null]]
    ['and', ['type' => Topic::TYPE], ['or', ['last_comment_username' => ''], ['last_comment_username' => null]]]
);
```

#### 嵌套查询

```php
$subQuery = new Query();
$subQuery->from(PostComment::tableName())->where(['status' => PostComment::STATUS_ACTIVE])->orderBy(['created_at' => SORT_DESC]);
$comment = PostComment::find()->from(['tmpA' => $subQuery])
    ->groupBy('post_id')
    ->all();
```

> 生成如下语句：`SELECT * FROM (SELECT * FROM`post_comment`WHERE`status`=1 ORDER BY`created_at`DESC)`tmpA`GROUP BY`post_id``

------

### 控制器相关

#### 获取模块/控制器/动作的id

```php
$this->module->id

$this->id

$this->action->id
```

#### 表单提交失败调试

```php
echo array_values($model->getFirstErrors())[0];exit;
```

#### 文件下载

存在一个文件地址，关于一些下载需要的header头信息Yii2已经帮我们完成，如下操作即可。

```php
public function actionDownload($id)
{
	$model = $this->findModel($id);

  	if ($model) {
  		// do something
  	}
  	return \Yii::$app->response->setDownloadHeaders($model->downurl);
}
```

> 模型的`downurl`属性可以通过 `extraFields()`进行设置。

#### 打印数据

```php
\yii\helpers\VarDumper::dump($var);

\yii\helpers\VarDumper::dump($var, 10 ,true);die; //  使用2  第二个参数是数组的深度  第三个参数是是否显示代码高亮（默认不显示）
```

#### 控制器调用其他控制器方法

```php
Yii::$app->runAction('new_controller/new_action', $params);
// 或者
return (new SecondController('second', Yii::$app->module))->runAction('index', $data);
```

#### 获取GET数据

```php
Yii::$app->getRequest()->get('id');
```

------

### 视图相关

#### 视图中获取当前模块/控制器/方法id

```php
Yii::$app->controller->module->id;

Yii::$app->controller->id

Yii::$app->controller->action->id
```

### 防止SQL注入或者XSS攻击

```php
echo yii\helpers\Html::encode($view_hello_str) // 可以原样显示<script></script>代码,但不解析 
echo yii\helpers\HtmlPurifier::process($view_hello_str)  // 可以过滤掉<script></script>代码 
```

------

### 配置相关

#### 用户组件登录修改

修改登陆状态超时时间（到期后自动退出登陆） `config/web.php`中的**components**组件数组中。

```php
'user' => [
	'class'=>'yii\web\User',
 	'identityClass' => 'common\models\User',
 	'loginUrl'=>['/user/sign-in/login'],
 	'authTimeout' => 1800, // 登陆有效时间
 	'as afterLogin' => 'common\behaviors\LoginTimestampBehavior'
],
```

#### 配置文件IP白名单

通过下面的IP地址方式配置debug的显示，便于调试代码。

```php
$config['modules']['debug'] = [
    'class' => 'yii\debug\Module',
    'allowedIPs' => ['127.0.0.1', '192.168.0.*', '192.168.33.1'],
];
```


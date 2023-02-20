### 1、函数的定义
```
func (recv receiver_type) methodName(parameter_list) (return_value_list) { 
    ... 
}
```

### 2、Go的大小写问题
go中根据首字母的大小写来确定可以访问的权限。
无论是方法名、常量、变量名还是结构体的名称，如果首字母大写，则可以被其他的包访问；
如果首字母小写，则只能在本包中使用。
可以粗暴的理解为首字母大写是公有的，首字母小写是私有的。
类属性如果是小写开头，则其序列化会丢失属性对应的值，同时也无法进行Json解析。

### 3、gorm 报错：using unaddressable value
```
func GetByDictType(dictType int) (blogDictList []*models.BlogDict) {
	fmt.Println(dictType)
	db := core.GetDb()
    //使用&传递指针
	db.Where(" dict_type = ?", dictType).Find(&blogDictList)
	return blogDictList
}
```
### 4、golang 中string和int类型相互转换
```
golang中字符串和各种int类型之间的相互转换方式：

string转成int：
int, err := strconv.Atoi(string)
string转成int64：
int64, err := strconv.ParseInt(string, 10, 64)
int转成string：
string := strconv.Itoa(int)
int64转成string：
string := strconv.FormatInt(int64,10)

```
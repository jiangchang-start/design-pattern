# 适配器模式
GOF定义：将一个类的接口适配成另外一个接口。Adapter模式使得原来由于接口不兼容而不能一起工作的类可以一起工作。做法是将类自己的接口包裹在一个已存在的类中。  

适配器模式类图
![](https://www.ibm.com/developerworks/cn/java/j-lo-adapter-pattern/img001.jpg)  
### 简单的例子
清单一.客户端需要使用的接口
```
/*
 * 定义客户端使用的接口，与业务相关
 */
public interface Target {
     /*
     * 客户端请求处理的方法
     */
    public void request();
}
```
清单二.被适配的对象
```
/*
 * 已经存在的类，这个类需要配置
 */
public class Adaptee {
     /*
     * 原本存在的方法
     */
    public void specificRequest(){
    //业务代码
    }
}
```
清单三.适配器实现
```
/*
 * 适配器类
 */
public class Adapter implements Target{
     /*
     * 持有需要被适配的接口对象
     */
    private Adaptee adaptee;
    /*
     * 构造方法，传入需要被适配的对象
     * @param adaptee 需要被适配的对象
     */
    public Adapter(Adaptee adaptee){
    this.adaptee = adaptee;
    }
    @Override
    public void request() {
    // TODO Auto-generated method stub
    adaptee.specificRequest();
    }
}
```
清单四.客户端代码
```
/*
 * 使用适配器的客户端
 */
public class Client {
    public static void main(String[] args){
        //创建需要被适配的对象
        Adaptee adaptee = new Adaptee();
        //创建客户端需要调用的接口对象
        Target target = new Adapter(adaptee);
        //请求处理
        target.request();
    }
}
```
### MyBatis Generator中Plugin中使用的例子
MyBatis Generator中提供了一个Plugin接口
清单五.Plugin接口
```
public interface Plugin {
    void setContext(Context var1);
    void setProperties(Properties var1);
    void initialized(IntrospectedTable var1);
    boolean validate(List<String> var1);
    List<GeneratedJavaFile> contextGenerateAdditionalJavaFiles();

    List<GeneratedJavaFile> contextGenerateAdditionalJavaFiles(IntrospectedTable var1);

    List<GeneratedXmlFile> contextGenerateAdditionalXmlFiles();

    List<GeneratedXmlFile> contextGenerateAdditionalXmlFiles(IntrospectedTable var1);

    boolean clientGenerated(Interface var1, TopLevelClass var2, IntrospectedTable var3);

    ...
}
```
清单六.PluginAdapter类
```
public abstract class PluginAdapter implements Plugin {
    protected Context context;
    protected Properties properties = new Properties();

    public PluginAdapter() {
    }

    public Context getContext() {
        return this.context;
    }

    public void setContext(Context context) {
        this.context = context;
    }

    public Properties getProperties() {
        return this.properties;
    }

    public void setProperties(Properties properties) {
        this.properties.putAll(properties);
    }

    public List<GeneratedJavaFile> contextGenerateAdditionalJavaFiles() {
        return null;
    }

    public List<GeneratedJavaFile> contextGenerateAdditionalJavaFiles(IntrospectedTable introspectedTable) {
        return null;
    }

    public List<GeneratedXmlFile> contextGenerateAdditionalXmlFiles() {
        return null;
    }
    ...
```
清单七.MapperAnnotationPlugin插件
```
public class MapperAnnotationPlugin extends PluginAdapter {
    public MapperAnnotationPlugin() {
    }

    public boolean validate(List<String> warnings) {
        return true;
    }

    public boolean clientGenerated(Interface interfaze, TopLevelClass topLevelClass, IntrospectedTable introspectedTable) {
        if (introspectedTable.getTargetRuntime() == TargetRuntime.MYBATIS3) {
            interfaze.addImportedType(new FullyQualifiedJavaType("org.apache.ibatis.annotations.Mapper"));
            interfaze.addAnnotation("@Mapper");
        }

        return true;
    }
}
```
任何人想要实现一个MyBatis Generator插件都只需要继承PluginAdapter类即可，同时由于PluginAdapter类给出了Plugin所有方法的默认实现，因此自己实现的插件只需要重写自己关注的方法即可，如清单七所示。我们可以认为Plugin接口对应Target，MapperAnnotationPlugin对应Adapter，这里面没有显式的Adaptee，可以将重写函数看做是Adaptee。

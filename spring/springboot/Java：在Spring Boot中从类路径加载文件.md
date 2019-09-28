# Java：在Spring Boot中从类路径加载文件

https://smarterco.de/java-load-file-from-classpath-in-spring-boot/

```
介绍
创建Spring Boot Web应用程序时，有时有时需要从类路径中加载文件，例如，如果数据仅作为文件提供。以我为例，我使用MAXMIND GeoLite2数据库进行地理位置检索。因此，我需要加载文件并创建一个DatabaseReader对象，该对象存储在服务器内存中。

在下面，您将找到在WAR和JAR中加载文件的解决方案。

资源加载器
使用Java，您可以使用当前线程的classLoader并尝试加载文件，但是Spring Framework为您提供了更为优雅的解决方案，例如ResourceLoader。

您只需要自动连接ResourceLoader，然后调用getResource(„somePath“)方法即可。

在Spring Boot（WAR）中从资源目录/类路径加载文件的示例
在下面的示例中，我们从类路径中加载名为GeoLite2-Country.mmdb的文件作为资源，然后将其作为File对象检索。

@Service("geolocationservice")
public class GeoLocationServiceImpl implements GeoLocationService {

    private static final Logger LOGGER = LoggerFactory.getLogger(GeoLocationServiceImpl.class);

    private static DatabaseReader reader = null;

    private ResourceLoader resourceLoader;

    @Autowired
    public GeoLocationServiceImpl(ResourceLoader resourceLoader) {
        this.resourceLoader = resourceLoader;
    }

    @PostConstruct
    public void init() {
        try {
            LOGGER.info("GeoLocationServiceImpl: Trying to load GeoLite2-Country database...");

            Resource resource = resourceLoader.getResource("classpath:GeoLite2-Country.mmdb");
            File dbAsFile = resource.getFile();

            // Initialize the reader
            reader = new DatabaseReader
                        .Builder(dbAsFile)
                        .fileMode(Reader.FileMode.MEMORY)
                        .build();

            LOGGER.info("GeoLocationServiceImpl: Database was loaded successfully.");

        } catch (IOException | NullPointerException e) {
            LOGGER.error("Database reader cound not be initialized. ", e);
        }
    }

    @PreDestroy
    public void preDestroy() {
        if (reader != null) {
            try {
                reader.close();
            } catch (IOException e) {
                LOGGER.error("Failed to close the reader.");
            }
        }
    }
}
从Spring Boot JAR加载文件
如果您想从Spring Boot JAR中的 classpath加载文件，则必须使用该resource.getInputStream()方法将其作为InputStream检索。如果尝试使用resource.getFile()该方法，则会收到错误消息，因为Spring尝试访问文件系统路径，但无法访问JAR中的路径。

@Service("geolocationservice")
public class GeoLocationServiceImpl implements GeoLocationService {

    private static final Logger LOGGER = LoggerFactory.getLogger(GeoLocationServiceImpl.class);

    private static DatabaseReader reader = null;
    private ResourceLoader resourceLoader;

    @Inject
    public GeoLocationServiceImpl(ResourceLoader resourceLoader) {
        this.resourceLoader = resourceLoader;
    }

    @PostConstruct
    public void init() {
        try {
            LOGGER.info("GeoLocationServiceImpl: Trying to load GeoLite2-Country database...");

            Resource resource = resourceLoader.getResource("classpath:GeoLite2-Country.mmdb");
            InputStream dbAsStream = resource.getInputStream(); // <-- this is the difference

            // Initialize the reader
            reader = new DatabaseReader
                        .Builder(dbAsStream)
                        .fileMode(Reader.FileMode.MEMORY)
                        .build();

            LOGGER.info("GeoLocationServiceImpl: Database was loaded successfully.");

        } catch (IOException | NullPointerException e) {
            LOGGER.error("Database reader cound not be initialized. ", e);
        }
    }

    @PreDestroy
    public void preDestroy() {
        if (reader != null) {
            try {
                reader.close();
            } catch (IOException e) {
                LOGGER.error("Failed to close the reader.");
            }
        }
    }
}
```



# springboot读取jar包resources下的文件

https://blog.csdn.net/qq_36223142/article/details/82660411



```
如果要在Spring Boot JAR中从类路径加载文件，则必须使用该resource.getInputStream()方法将其作为InputStream进行检索。如果您尝试使用，resource.getFile()您将收到错误，因为Spring尝试访问文件系统路径，但它无法访问JAR中的路径。

//方式1：
    InputStream inputStream =  getClass().getClassLoader().getResourceAsStream(fileName)
 
 
 
//方式2：
    public static String getFile(String filePath) {
        ResourceLoader resourceLoader = new DefaultResourceLoader();
        Resource resource=resourceLoader.getResource("classpath:"+filePath);
        StringBuffer strBuff = new StringBuffer();
        try (InputStream inputStream = resource.getInputStream()) {
            BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
            String string = null;
            while ((string = bufferedReader.readLine()) != null) {
                strBuff.append(string);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return strBuff.toString();
    }
参考：https://smarterco.de/java-load-file-from-classpath-in-spring-boot/
————————————————
版权声明：本文为CSDN博主「咬瓶盖」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_36223142/article/details/82660411
```





# springboot打成jar包后读取配置resources下的文件（已解决）

https://blog.csdn.net/cscscssjsp/article/details/84822706



spring boot 项目读取自定义配置文件的坑
springboot读取配置文件
1.以下是亲测成功示例打成jar包放到linux服务器跑
2.这个是原来的写法，打成jar包放到linux服务器后找不到配置文件，会报错
springboot读取配置文件
一般情况下我们通过ResourceUtils.getFile(“classpath:config.json”)就可以读取自定义的配置文件
如果是打war包后也可以读取，但是如果你打的是jar包就不可以，jar包找不到classpath的路径

1.以下是亲测成功示例打成jar包放到linux服务器跑
InputStream inputStream = Thread.currentThread().getContextClassLoader().getResourceAsStream("config.json");
		String configContent =  this.readFile( inputStream );
		System.out.println(configContent);
		
1
2
3
4
readFile是自定义的一个函数，用来处输入流返回一个字符串

/*
	 * 读取配置文件
	*/	
	private String readFile ( InputStream inputStream ) throws IOException {
		
		StringBuilder builder = new StringBuilder();
		try {
			InputStreamReader reader = new InputStreamReader(inputStream , "UTF-8" );
			BufferedReader bfReader = new BufferedReader( reader );
			String tmpContent = null;
			while ( ( tmpContent = bfReader.readLine() ) != null ) {
				builder.append( tmpContent );
			}
			bfReader.close();
		} catch ( UnsupportedEncodingException e ) {
			// 忽略
		}
		return this.filter( builder.toString() );
	}
	// 过滤输入字符串, 剔除多行注释以及替换掉反斜杠
	private String filter ( String input ) {
		return input.replaceAll( "/\\*[\\s\\S]*?\\*/", "" );
	}
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
2.这个是原来的写法，打成jar包放到linux服务器后找不到配置文件，会报错
String configPath = ResourceUtils.getFile("classpath:config.json").getAbsolutePath();
String configContent =  this.readFile( configPath );
1
2
之前的readFile是这样的

/*
	 * 读取配置文件
	*/	
	private String readFile ( String path ) throws IOException {
		
		StringBuilder builder = new StringBuilder();
		try {
			InputStreamReader reader = new InputStreamReader(new FileInputStream(path) , "UTF-8" );
			BufferedReader bfReader = new BufferedReader( reader );
			String tmpContent = null;
			while ( ( tmpContent = bfReader.readLine() ) != null ) {
				builder.append( tmpContent );
			}
			bfReader.close();
		} catch ( UnsupportedEncodingException e ) {
			// 忽略
		}
		return this.filter( builder.toString() );
	}
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
3.网上百度一波，还有另外的操作，但是没试验打jar可不可以

InputStream inputStream = new ClassPathResource("config.json").getInputStream();
..................
// 如上成功示例，调用readFile方法处理流就行了
1
2
3
这个本地测试也能成，没有打包测试

InputStream inputStream = this.getClass().getClassLoader().getResourceAsStream("config.json");
..................
// 如上成功示例，调用readFile方法处理流就行了
1
2
3
总结，好坑！！！
————————————————
版权声明：本文为CSDN博主「Javacssjsp」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/cscscssjsp/article/details/84822706





```
想读取resouce/picture下的bottom.png文件。只有第二种方式才能正常工作！

方式一：
File sourceFile = ResourceUtils.getFile("classpath:picture/bottom.png"); //这种方法在linux下无法工作
方式二：
Resource resource = new ClassPathResource("picture/bottom.png");
File sourceFile =  resource.getFile();
————————————————
版权声明：本文为CSDN博主「Big Bird」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/ruren1/article/details/85165169
```


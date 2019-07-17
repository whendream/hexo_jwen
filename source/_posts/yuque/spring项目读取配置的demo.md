
---

title: spring项目读取配置的demo

date: 2019-04-16 15:54:56 +0800

tags: []

---
# 背景
读取配置是基础能力，研发这个模式不错，可以从不同配置中读取数据，如下图：  
![](https://img2018.cnblogs.com/blog/411616/201901/411616-20190105115713584-54200896.png)  
可以根据不同分类的文件来管理配置，然后统一在conf中配置哪些文件

    package com.jwen.platform.config;
    
    import com.jwen.platform.exception.AppConfigurationException;
    
    import java.io.BufferedReader;
    import java.io.IOException;
    import java.io.InputStream;
    import java.io.InputStreamReader;
    import java.net.URISyntaxException;
    import java.util.ArrayList;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import java.util.regex.Pattern;
    
    public enum Configurations {
        INSTANCE;
    
        private final static String KEY_VALUE_SEPARATOR = "=";
        private final static String CONFIGURATION_FILE = "application.conf";
        private final static String OTHER_CONFIGURATION_FILE_KEY = "app_include_file";
    
        private List<String> otherConfigurationFiles = new ArrayList<>();
        private Map<String, String> configurations = new HashMap<>();
    
        Configurations() {
            try {
                init();
            } catch (IOException | URISyntaxException e) {
                throw new AppConfigurationException("has error in your application.conf", e);
            }
        }
    
        private void init() throws IOException, URISyntaxException {
    
            InputStream resourceAsStream = this.getClass().getClassLoader().getResourceAsStream(CONFIGURATION_FILE);
            BufferedReader br = new BufferedReader(new InputStreamReader(resourceAsStream));
    
            String s = "";
            List<String> lines = new ArrayList<String>();
    
            while ((s = br.readLine()) != null) {
                lines.add(s);
            }
    
            
            resourceAsStream.close();
            br.close();
    
            initConfigurations(lines);
            initOtherConfigurations();
    
        }
    
        private void initOtherConfigurations() throws URISyntaxException, IOException {
            for (String fileName : otherConfigurationFiles) {
                InputStream resourceAsStream = this.getClass().getClassLoader().getResourceAsStream(fileName);
                BufferedReader br = new BufferedReader(new InputStreamReader(resourceAsStream));
    
    
                String s = "";
                List<String> lines = new ArrayList<String>();
    
                while ((s = br.readLine()) != null) {
                    lines.add(s);
                }
    
                
                resourceAsStream.close();
                br.close();
    
    
                initConfigurations(lines);
            }
    
            otherConfigurationFiles = null;
        }
    
        private boolean isOtherConfigurationKey(String key) {
            return OTHER_CONFIGURATION_FILE_KEY.equalsIgnoreCase(key);
        }
    
        private void initConfigurations(List<String> lines) throws URISyntaxException, IOException {
            lines.stream().forEach(line -> {
                int keyValueSeparatorIndex = line.indexOf(KEY_VALUE_SEPARATOR);
                if (isLegalConfigurationLine(line)) {
                    String key = line.substring(0, keyValueSeparatorIndex).trim();
                    String value = line.substring(keyValueSeparatorIndex + 1, line.length()).trim();
                    if (isOtherConfigurationKey(key)) {
                        otherConfigurationFiles.add(value);
                    } else {
                        configurations.put(key, value);
                    }
                }
            });
        }
    
        private boolean isLegalConfigurationLine(String line) {
            Pattern pattern = Pattern.compile("^[a-zA-Z](.*?)=(.*?)");
            return pattern.matcher(line).matches();
        }
    
    
        public String get(String key) {
            return configurations.get(key);
        }
    
        public static void main(String[] args) {
    
            System.out.println(Configurations.INSTANCE.get("test.on"));
        }
    }

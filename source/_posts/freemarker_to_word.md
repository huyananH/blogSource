---
title: FreeMarker转Word
date: 2017-10-20 22:07:19
comments: true
reward: true
tags:
  -FreeMarker
  -Word
  -Java
---

<img src="/assets/postLog/freeMarkerToWordLog.jpeg" width="350px" height="350px">

### 1. 前言

朋友今天说，和我聊天全靠猜......

他说聊天不是做阅读理解、完型填空......

<!-- more -->

### 2. 新建一个Word，调成你想要的格式，好比下边这张图
<img src="/assets/postImg/wordExa.jpg" width="400px" height="300px">

### 3. 把创建好的word文件，另存为XML文件
<img src="/assets/postImg/xmlformat.jpg" width="400px">

### 4. 新建一个index.ftl文件，把XML文件中的内容复制到index.ftl文件中

### 5. 写代码，使用刚才的index.ftl文件生成word

*—— 详细代码[点击这里](https://github.com/huyananH/FreeMarkerToWord.git)*
```
@Autowired
private PatientCheckPumchService patientCheckPumchService;

@Qualifier("freeMarkerConfiguration")
@Autowired
private Configuration configuration;

//批量下载
@ApiOperation(value = "下载孩子病例文档")

@RequestMapping(value = "/batchDownload", method = RequestMethod.GET)
public void batchDownload(@RequestParam  List<String> informationIds,
                          HttpServletRequest request,
                          HttpServletResponse response) throws TemplateException, UnsupportedEncodingException {
    request.setCharacterEncoding("UTF-8");
    DateFormat dateFormat = new SimpleDateFormat("yyyyMMddHHmmss");
    String sb = dateFormat.format(new Date());
    StringBuilder docName = new StringBuilder();
    for (String id : informationIds) {
        docName.append(id);
    }

    try {
        File wordFile = new File("word/");
        if (!wordFile.exists()) {
            wordFile.mkdir();
        }
        File file = new File("word/"+sb);

        List<File> srcfile=new ArrayList<File>();
        for (String id : informationIds) {
            List<PatientCheckPumchExtend> extend = patientCheckPumchService.findByInforId(Long.parseLong(id));
            Map<String, Object> map = new HashMap<>();
            map.put("extend",extend);

            configuration = new Configuration(Configuration.getVersion());
            configuration.setDefaultEncoding("UTF-8");
            configuration.setDirectoryForTemplateLoading( new File("src/main/resources/templates"));
            Template t = configuration.getTemplate("wordSwitch.ftl");

            if (!file.exists()) {
                file.mkdir();
            }
            String outNewFileName = "word/"+sb + "/" + extend.get(0).getPatientInformationPumchExtend().getOid()+".doc";
            File outNewFile = new File(outNewFileName);
            if (!outNewFile.exists()) {
                outNewFile.createNewFile();
            }
            PrintWriter out = new PrintWriter(new OutputStreamWriter(new FileOutputStream(outNewFile), "UTF-8"));
            t.process(map, out);
            out.flush();
            out.close();
        }

        if (informationIds.size()==1) {
            FileUtils.downFile(response,"word/"+sb+"/", docName+".doc");
        }else {
            File zipFile = new File("word/"+sb+".zip");
            FileUtils.zipFiles("word/"+sb,zipFile);
            FileUtils.downFile(response,"word/", sb+".zip");

        }
    }catch (Exception e) {
        e.printStackTrace();
    }
}
```

# File  
## 1.文件声明  
File(String pathname) 
File(String parent, String child)
File(File parent, String child)  
*两个参数的第一个参数传的是父类地址*   

**注：有一个地址D：\1.txt，此地址不能扔到File参数里面，因为\x代表转移符，会发生编译错误，可以声明为两个\\**  

**对File的理解可以理解为指针，虽然传入了地址的参数，但这并不能代表什么**    
## 2.文件方法
### 1.创建功能
**public boolean createNewFile()**  
**public boolean mkdir()**  
**public boolean mkdirs()**  

#### -public boolean createNewFile()
此方法要try catch，因为可能：
- 文件名，目录名，语法不对（比如只有\）
- 磁盘已满
- 目录不存在    

**注：new（D:\\1.text）是new一个txt，但new（D：\\www）并不是new一个文件夹，new文件夹需要用到如下方法**  

#### -public boolean mkdir()  
**注：如果有一个文件名叫www，而我们用mkdir方法new一个叫做www的文件夹，那么不能创建这个文件夹**  
**mkdir不能创建多级目录，需要用如下方法**  
#### -public boolean mkdirs（）  
加一个s代表多级目录  
### 2.删除功能
**public boolean delete()**  
file.delete是删除某一个文件或者文件夹，并不能删除多级文件夹  
**删除文件后文件不会出现在回收站中**  
### 3.重命名功能  
**public boolean renameTo(File dest)**  
**简而言之，操作实质为剪贴并改名**
### 4.判断功能  
**public boolean isFile()**  
**public boolean isDirectory()**  
**public boolean exists()**  
**public boolean canRead()**  
**public boolean canWrite()**  
**public boolean isHidden()**    

#### - public boolean isFile（）
判断是否为文件，若为文件夹，返回false
#### - public boolean isDirection（）
判断是否为文件夹，若为文件，返回false
#### - public boolean exists（）
若文件或文件夹存在，返回true  
**注：一般先判断exists，再判断isFile或isDirection（）**
#### -public boolean canRead()**
是否可读
#### - public boolean canWrite（）
是否可写
#### - public boolean isHidden（）
是否隐藏  

### 5.基本获取功能
**public File getAbsoluteFile()**  
**public String getAbsolutePath（）**  
**public String getPath()**  
**public String getName()**  
**public long length()**  
**public long lastModified()**    
> 相对路径：相当于java执行程序的目录，若File（“1.txt”）此路径就是相对路径，会在workspace下的项目名目录下产生一个1.txt的文件，与src并列    
  
  
#### - public String getAbsolutePath（）
获取绝对路径
#### - public String getPath（）
获取相对路径
**注：前提是文件是以相对路径新创建的，若为绝对路径创建的文件，则获取的是绝对路径** 
#### -public File getAbsoluteFile（）
**获取的还是当前文件，只是文件若是以相对路径生产的，则会获得绝对路径**
#### -public String getName（）
返回当前文件名
#### -public long length（）
返回当前文件字节数**byte**  
#### -public long lastModified（）
返回1970年1月1日0点到当前时间的秒数    
> data.toString———————只是返回当前日期  
> data.toGMTString—————返回目前本初子午线出的时间（精确到秒级）  
> data.toLoaclString—————返回当地时间（北京时间，精确到秒级）  
    
### 6.高级获取功能
**public String[] list()**  
**public File[] listFiles()**  
**public String[] list(FilenameFilter filter)**  


#### -public String[]) list()
**返回的仅仅是当前目录下文件名的数组，若当前目录有文件文件夹，不会囊括此文件夹下的文件名**

#### - public File[] listFiles()
**威力加强版，返回的是当前目录下的file的数组，可遍历出多种操作，如名字，大小等（getName，length），同样不会囊括文件夹下的文件名**  

#### - public String[] list(FileFileter filter)  
```
File[] listFiles = file.listFiles(new FileFilter() {
@Override
  public boolean accept(File file) {
    return file.getName().endsWith(".txt");
}
});
```  
一个匿名内部类，将一个重写了accept方法的过滤器扔到里面
```
    public File[] listFiles(FileFilter filter) {
        String ss[] = list();
        if (ss == null) return null;
        ArrayList<File> files = new ArrayList<>();
        for (String s : ss) {
            File f = new File(s, this);
            if ((filter == null) || filter.accept(f))
                files.add(f);
        }
        return files.toArray(new File[files.size()]);
    }
```  


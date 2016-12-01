package wordcount;

import java.io.IOException;
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.GenericOptionsParser;

public class Single
{

public static class Map extends Mapper<Object, Text, Text, Text>
{
    //从输入中得到的每行的数据的类型
    private static Text line = new Text();

    //实现map函数
    public void map(Object key, Text value, Context context) throws IOException, InterruptedException
    {
        //获取并输出每一次的处理过程
        String line = value.toString();
        String StrIp[] = line.split("\t");
        System.out.println("The process of the Map:" + StrIp[1]);
        context.write(new Text(StrIp[0]), new Text(""));
    }
}

//reduce将输入中的key复制到输出数据的key上，并直接输出
public static class Reduce extends Reducer<Text, Text, Text, Text>
{

    //实现reduce函数
    public void reduce(Text key, Iterable<Text> values, Context context)
    throws IOException, InterruptedException
    {
        //获取并输出每一次的处理过程
        System.out.println("The process of the Reduce:" + key);
        context.write(key, new Text(""));
        
    }

}

public static void main(String[] args) throws Exception
{

    //设置配置类
    Configuration conf = new Configuration();

    //是从命令行里获取输入数据和输出数据的路径，所以这里要获取和判断一下
   
    Job job = new Job(conf, "Date-Single");
//    Job job = Job.getInstance();
//    job.setJobName("single");
    job.setJarByClass(Single.class);
    //设置Map、Combine和Reduce处理类
    job.setMapperClass(Map.class);
    job.setCombinerClass(Reduce.class);
    job.setReducerClass(Reduce.class);
    //设置输出类型
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(Text.class);
    //设置输入和输出目录
    FileInputFormat.addInputPath(job, new Path(args[0]));
    FileOutputFormat.setOutputPath(job, new Path(args[1]));
   
    System.exit(job.waitForCompletion(true) ? 0 : 1);

}
}

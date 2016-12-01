
public class IpReadme {
	public static  class  IndexMapper extends Mapper<LongWritable, Text,Text, IntWritable>{
		private Set<String> set1 = new HashSet<String>();
		public void Mapper(LongWritable key,Text value,Context context) throws IOException, InterruptedException{
			String line =value.toString();
			String str[] =line.split("\t");
			FileSplit fileSplit=(FileSplit) context.getInputSplit();
			String path=fileSplit.getPath().toString();
          if(path.contains("ip_time")){
          	set1.add(fields[0]);
           }else{
             	set2.add(fields[0]);
            }
 	}
			
		}
	}
	public static class IndexReducer extends Reducer<Text, IntWritable, Text, IntWritable>{

		protected void reduce(Text key, Iterable<IntWritable> values,Context context)
				throws IOException, InterruptedException {
			// TODO Auto-generated method stub
			 Set<String> set = new HashSet<String>();
			 

	
		}
		
	}
	public static void main(String[] args) throws Exception {
		 Configuration conf = new Configuration();

		    Job job = new Job(conf, "Date-Single");
		    job.setJarByClass(Single.class);
		    //设置Map、Combine和Reduce处理类
		    job.setMapperClass(Map.class);
		    job.setCombinerClass(Reduce.class);
		    job.setReducerClass(Reduce.class);
		    //设置输出类型
		    job.setOutputKeyClass(Text.class);	
		    job.setOutputValueClass(Text.class);

		    FileInputFormat.addInputPath(job, new Path(args[0]));
		    FileOutputFormat.setOutputPath(job, new Path(args[1]));

		    System.exit(job.waitForCompletion(true) ? 0 : 1);

		}
}

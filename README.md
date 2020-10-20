//Highest sales branch wise C#
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
namespace ProgramNamespace
{
    public class Program
    {
        public static List<String> processData(IEnumerable<string> lines)
        {
            List<string>branchList=new List<string>(lines);
            string salesString="";
            string[] branchArray= new string[branchList.Count()];
            int[] priceArray= new int[branchList.Count()];
            for (int i = 0; i < branchList.Count(); i++){ 
                branchArray[i]=branchList[i].Split(',').ToList()[1].Trim().ToString();
                priceArray[i]=Convert.ToInt32(branchList[i].Split(',').ToList()[4].Split(' ')[2].Trim().ToString());
            } 
            Array.Sort(priceArray,branchArray);
            string[] collectionWithDistinctBranchs=branchArray.Distinct().ToArray();
            int[] collectionWithDistinctPrices= new int[branchList.Count()];
            for(int i=0; i<collectionWithDistinctBranchs.Count();i++){
                for(int j = 0; j <branchList.Count() ; j++){ 
                    if(collectionWithDistinctBranchs[i]==branchArray[j]){
                        collectionWithDistinctPrices[i]=priceArray[j];
                    }
                }
            }
            string[] resultArray= new string[branchList.Count()];           
            int[] collectionWithResultePrices= new int[branchList.Count()];
            int c=0;
            for(int i = 0; i < branchList.Count(); i++){ 
                for(int j=0; j<collectionWithDistinctBranchs.Count();j++){
                    if(branchList[i].Split(',').ToList()[1].Trim().ToString()==collectionWithDistinctBranchs[j] && Convert.ToInt32(branchList[i].Split(',').ToList()[4].Split(' ')[2].Trim().ToString())==collectionWithDistinctPrices[j]){
                        if(!resultArray.Contains(branchList[i].Split(',').ToList()[0].Trim().ToString())){
                            resultArray[c]=branchList[i].Split(',').ToList()[0].Trim().ToString();
                            collectionWithResultePrices[c]=collectionWithDistinctPrices[j];
                            c+=1;
                            break;
                        }
                    }
                }
            } 
            Array.Sort(collectionWithResultePrices,resultArray);
            List<String>retVal=new List<String>(resultArray);
            retVal.Reverse();
            return retVal;
        }
        static void Main(string[] args)
        {
            try
            {
                String line;
                var inputLines = new List<String>();
                while((line = Console.ReadLine()) != null) {
                  line = line.Trim();
                  if (line != "")
                    inputLines.Add(line);
                }
                var retVal = processData(inputLines);
                foreach(var res in retVal)
                  Console.WriteLine(res);
            }
            catch (IOException ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    }
}

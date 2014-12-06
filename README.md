Kosaraju-s-Two-Pass-Algorithm
=============================
Programming assignment - 4 
Coursera.com
Algorithms: Design and Analysis, Part 1
Program to compute Strongly Connected components in a graph using Kosaraju's Two-Pass Algorithm

```
import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.TreeSet;

public class SCC {

    //global variables 
    HashMap<Long, Boolean> explored = new HashMap<Long, Boolean>();
    HashMap<Long, ArrayList<Long>> vertices = new HashMap<Long, ArrayList<Long>>();
    TreeSet<Long> sizes = new TreeSet<Long>();
    HashMap<Long, Long> ftime = new HashMap<Long, Long>();
    long t=0,s=0,n=875715;
    
    // method to intialise the data structures 
    public void initialise() {
        explored.clear();
        vertices.clear();
        System.out.println("cleared");
        for(long j=1; j<n ; j++ ) {
            ArrayList<Long> adjacent=new ArrayList<Long>();
            vertices.put(j,adjacent);
            explored.put(j, false);
        }
    }
        
    // method to load vertices 
    public void loadVertices(long m) {
        System.out.println("Loading began");
    	try{
            
            File myFile=new File("SCC.txt");
            BufferedReader reader = new BufferedReader( new FileReader(myFile) ) ;
            String line=null;

            while( (line=reader.readLine()) != null) {
                String[] token = line.split(" ") ;
                
                ArrayList<Long> adjacent = vertices.get( Long.parseLong( token[(int)(0.5 - (m*0.5))] ) ) ;
                adjacent.add( Long.parseLong( token[(int)(0.5 + (m*0.5))] ) );
                
                //System.out.println(token[0]+"--->"+token[1]);
                // no need to put it in graph again ;)
            }
            reader.close() ;
        }
        catch(IOException e) {
            System.out.println("Bummer");
        }
        System.out.println(" Done Loading " );
        System.out.println(vertices.get((long)1)+" "+vertices.get((long)2));
    }
    
    // DFS loop
    public void DFS(long i, boolean first) {
        s++;
        if(!first){
        	System.out.print(" "+i);
        }
        explored.put(i, true);
        ArrayList<Long> adjacent = vertices.get(i) ;
        for( int j=0 ; j<adjacent.size() ; j++) {
            if( !(explored.get( adjacent.get(j) ))) {
                DFS(adjacent.get(j), first);
            }
        }
        if(first) {
            t++;
            ftime.put(t, i);
        }
    }
        
    //main
    public static void main(String[] args) {
        System.out.println("Starting");
        SCC coursera = new SCC();
        coursera.go() ;
    }
    
    // go .. all stuff happens here 
    public void go() {
    
        // setting-up
        initialise();
        loadVertices(-1);
        System.out.println("First call started");
        
        // first outer loop .. will decide finishing times 
        for(long j=n-1; j>0 ; j-- ) {
            if( !(explored.get(j)) ) {
                DFS(j, true);
            }
        }
        System.out.println(" 1st run over \n ftime size = " +ftime.size() );
        System.out.println(t);
        
        //to reverse the graph 
        initialise();
        loadVertices(1);
        System.out.println("Second call started");
        
        // second outer loop
        for(long j=n-1;j>0;j--) {
            if( !(explored.get((long) ftime.get(j))) ) {
                s=0;                
                DFS( ftime.get(j), false);                
                sizes.add(s);

            }
        }
        
        // printing the sizes of SCCs
        System.out.println("\n"+ sizes );
    }
}
        
        
```

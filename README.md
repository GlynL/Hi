# Hi

#include <cs50.h>
#include <stdio.h>

int main(int argc, char *argv[])
{
    
    //check proper command line usage
    if (argc != 2)
        {
            fprintf(stderr, "Usage: ./recover FILE\n");
            return 1;
        }


    //open input file
    FILE *inptr = fopen(argv[1], "r");
    

    //check for valid file
    if(inptr == NULL)
        {
            fprintf(stderr, "Invalid File %s.\n", argv[1]);
            return 2;
        }
        
        
    //declare file for images
    FILE *img = NULL;
    
    //counter for jpg name
    int i = 0;
    
    
    //loop until end of file found
    do{
        
        
        
        //read input file
        unsigned char store[512];
        fread(store, 512, 1, inptr);
        
        
        //check for start of JPEG
         
         if (store[0] == 0xff &&
            store[1] == 0xd8 &&
            store[2] == 0xff &&
            (store[3]  &0xf0) == 0xe0)
            
            {
            
                //check if alrady found a JPEG
            
                if (img == NULL)
                {
                    
                    //print string for jpg name
                    char filename[50];
                    sprintf(filename, "%03i.jpg", i);
                    
                
                    //open jpg for output
                    img = fopen(filename, "w");
                
                
                    //write to new jpg
                    fwrite(store, 512, 1, img);
                }
            
            
                else
                {
                    //close open jpg
                    fclose(img);
                
                
                    //add to jpg name
                    i++;
                
                
                    //print string for jpg name
                    char *filename = NULL;
                    sprintf(filename, "%03i.jpg", i);
                
                
                    //open jpg for output
                    img = fopen(filename, "w");
                
                
                    //write to new jpg
                    fwrite(store, 512, 1, img);
                }
            }
        
            
        else
        {
            //check if already found jpg
            if(img == NULL)
            {
                //exit loop to check next 512 bytes
                break;
            }
            
            else
            {
                
                //write to open jpg
                fwrite(store,512, 1, img);
                
            }
            
        }
                        
        
        }
        while (fread >0);
        
        
     //close input file 
     fclose(inptr);
     
     //close jpg file
     fclose(img);
     
     
     
     //return success
     return 0;
    
}

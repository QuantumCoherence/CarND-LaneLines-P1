### How To run the code
This is a Jupyter Notebook, run all cells one by one from top to bottom 
 
###BEWARE:
TO RUN the pipeline on the videos

COMMENT OUT THE FIRST TWO LINES of code of cell 39, 

starting with the "def pipeline" line of code and delete the comment # on the third line

	def pipeline(imagefile):
    	image = mpimg.imread(imagefile)   
	#def pipeline(image):

Respectively, comment in the first two lines of code and out the third one before attempting to test 
the pipeline on any of the single images (cell numbers 29 to 36)

	#def pipeline(imagefile):
    #	image = mpimg.imread(imagefile)   
	def pipeline(image):

### Videos

Processed video are to be found in the root folder of the project named
 
solidWhiteRightOut.mp4

solidYellowLeftOut.mp4

challengeOut.mp4



The script first check it the data exists and the download its to a folder named 'Data'

Then it reads all the .txt data (Activity, training and features) and merge them

After it, the scrip merges all data and extracts extract all the sd() and mean() measurements of the datase, obtaning an unique dataset without labels on the activiy column

So the script procedes to read the activity value labels on "activity_labels.txt" and applies them to the activity variable

There is no change in the name of all variables as they are currently undestandable

From the data set, I created a second, independent tidy data set with the average of each variable for each activity and each subject.

And as there are 30 subjects and 6 activities, the new data frame should have 180 obs


Prefixes in Variables:

prefix t is time
Acc is Accelerometer
Gyro is Gyroscope
prefix f is frequency
Mag is Magnitude
BodyBody is Body

## This file explains how the script works:

setwd("~/R Studio personal projects/Curso Johns Hopkins/Getting and Cleaning Data/Final Project")

if(!file.exists("./data")){dir.create("./data")}
fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileUrl,destfile="./data/ProjectDataset.zip",method="curl")

unzip(zipfile="./data/ProjectDataset.zip",exdir="./data")

datapath <- file.path("./data" , "UCI HAR Dataset")

## Reading activity, training and features data

ActivityTest<- read.table(file.path(datapath, "test" , "y_test.txt" ),header = FALSE)
ActivityTrain<- read.table(file.path(datapath, "train", "y_train.txt"),header = FALSE)
SubjectTest<- read.table(file.path(datapath, "test" , "subject_test.txt"),header = FALSE)
SubjectTrain<- read.table(file.path(datapath, "train", "subject_train.txt"),header = FALSE)
FeaturesTest<- read.table(file.path(datapath, "test" , "X_test.txt" ),header = FALSE)
FeaturesTrain<- read.table(file.path(datapath, "train", "X_train.txt"),header = FALSE)

## Merging Activity with Training dataframes

Activity<- rbind(ActivityTrain, ActivityTest)
Subject <- rbind(SubjectTrain, SubjectTest)
Features<- rbind(FeaturesTrain, FeaturesTest)

# Creating names for columns (variables)

names(Subject)<-c("subject")
names(Activity)<- c("activity")
FeaturesNames <- read.table(file.path(datapath, "features.txt"),head=FALSE)
names(Features)<- FeaturesNames$V2

## Merging all data to create one dataset

data<- cbind(Subject, Activity)
data<- cbind(Features, data)

##extracting the mean and ST.dev for each measurement
subFeaturesNames<-FeaturesNames$V2[grep("mean\\(\\)|std\\(\\)", FeaturesNames$V2)]
selectNames<-c(as.character(subFeaturesNames), "subject", "activity" )
data<-subset(data,select=selectNames)

str(data)

## adding labels to the activy column

Labels <- read.table(file.path(datapath, "activity_labels.txt"),header = FALSE)
data$activity <- factor(data$activity, levels = c(1,2,3,4,5,6),labels = c("WALKING", "WALKING_UPSTAIRS", "WALKING_DOWNSTAIRS", "SITTING", "STANDING", "LAYING"))
head(data$activity)

## 4. Appropriately labels the data set with descriptive variable names.
##honestly I did not found any descriptive variable name, neither on the website or the zipfile :(


## From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.


library(plyr)
data2<-aggregate(. ~subject + activity, data, mean)

## putting subjetc and activity first in the new data set
data2<-data2[order(data2$subject,data2$activity),]

## there are 30 subjects and 6 activities, so the new data frame should have 180 obs
write.table(data2, file = "tidydata.txt",row.name=FALSE)

# Getting_and_Cleaning_Assignment
Assignment for Getting and Cleaning data

#I first put the text files needed for assignment in a separate folder then made it the directory of my script
#I didn't show this to just make it easy to see the real code 
#here are the steps......

## read in all test and train tables

subject_test <- read.table("subject_test.txt")
subject_train <- read.table("subject_train.txt")
X_test <- read.table("X_test.txt")
X_train <- read.table("X_train.txt")
y_test <- read.table("y_test.txt")
y_train <- read.table("y_train.txt")
activity_labels <- read.table("activity_labels.txt")[,2]
features <- read.table("features.txt")[,2]

## give column names to X_test and X_train

names(X_test) = features
names(X_train) = features
y_test[,1] = activity_labels[y_test[,1]]
y_train[,1] = activity_labels[y_train[,1]]

## combine the test tables to make one large test table then do same with train

fulltest <- cbind(subject_test,y_test,X_test)
fulltrain <- cbind(subject_train,y_train,X_train)

## combine both tables using the columns and make one large table, since cbinds were ordered in same way 
## no need to worry about columns being mixed

testandtrain <- rbind(fulltrain,fulltest)

## get only mean and std names

meanstd <- testandtrain[,grepl("mean|std|V1", colnames(testandtrain))]

## descriptive label names....

colnames(meanstd)[2] <- "Activity"
colnames(meanstd)[1] <- "Subject"

## create tidy data set with the average of each variable for each activity and each subject

melt_data <- melt(meanstd, id.vars = c("Subject","Activity"))
tidy_data = dcast(melt_data, Subject + Activity ~ variable, mean)

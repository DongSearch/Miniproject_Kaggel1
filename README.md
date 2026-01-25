## 🎯 Goal: 


## 🛠 Algorithms Used:
- AdamW
### config
- epoch : 10 -> 50
- batchsize : 128->64->32
### DataAugumentation
- RandomHorizontalFlip
- RandomRotation
- ColorJitter
- RandomErasing
### Model(Resnet18)
- fine-tuning layer : 7x7 kernel -> 3x3 kernel
- last layer : output 1000 -> 10
- Earlystopping
- warmup scheduler(Linear)
- main scheudler(ConsineAnnealingLR)
- label smoothing 
## 📅 Key milestone(summary)
- **Jan 23:** running basic code & analysis principal problems that contribute to low performance
- **Jan 24:** change SGD to AdamW,add Data Augumentation, increase epochs, chagne first layer of model
- **Jan 25:** add Earlystopping, scheuduler(warmup + main), masking(data pre-prossing), reduce batch-szie, increas epoch



## 🧠 Diary(Detail)
### Trial(Jan 23)
### result
- running basic code(accuracy 38%)
<img width="1590" height="490" alt="image" src="https://github.com/user-attachments/assets/b56126f4-ac56-489b-af96-dd5aef532f2b" />

### problem
- low accuracy in both training and validation
### Analysis
- less sample(training 5000, test 8000) -> DataAugumentaion is needed
- less epoch(only 5) -> increase the number of epochs
- optimizer(SGD) -> Adam


## 🧠 Diary(Detail)
### Trial(Jan 24)
- change SGD to AdamW
- add Data Augumentation(hoizeontalflip, randomrotation,colorjitter)
- increase epochs(5 -> 10) : too less epochs
- chagne first layer of model(resnet18 is targeted to (224,224) images while ours are (96,96), so reduce kernersize( 7x7 -> 3x3), deactivate maxpooling)
### result
- improve the accuracy(54.8%)
<img width="1590" height="490" alt="image" src="https://github.com/user-attachments/assets/d68b5144-aa40-44ea-a34b-86ca2a14acc8" />


### problem(low accuracy)
- still less accuracy
### Analysis
- increase epochs with more diverse augumentation, especially Masking(still looks like accuracy is soaring
- need to figure out which type of classs or images get error mostly
- might reduce the size of kernel
-  

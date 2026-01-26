## 🎯 Goal: 
we need to improve performance of our custome model with small image datesets(5k,3x96x96, class:10(balanced dataset)

## 🛠 Algorithms Used:
- AdamW
### config
- epoch : 10 -> 50 -> 100
- batchsize : 128->64->32->64
### DataAugumentation
- RandomHorizontalFlip
- RandomRotation
- ColorJitter
- RandomErasing
- Masking(Erasing)
- MixUp / CutMix
### Model(Resnet18)
- custom Resnet(same as 2blocks but different scale : 32-64-128-256, add dropout) 
-  fine-tuning layer : 7x7 kernel -> 3x3 kernel ❌ (to heavy 11M parameters)
- last layer : output 1000 -> 10 ❌ 
- Earlystopping
- warmup scheduler(Linear)
- main scheudler(ConsineAnnealingLR)
- label smoothing 
## 📅 Key milestone(summary)
- **Jan 23:** running basic code & analysis principal problems that contribute to low performance
- **Jan 24:** change SGD to AdamW,add Data Augumentation, increase epochs, chagne first layer of model
- **Jan 25:** add Earlystopping, scheuduler(warmup + main), masking(data pre-prossing), reduce batch-size, increas epoch
- **Jan 26:** add cutmix/mixup, change to custom resnet, increase batch size, no improvement of accuracy



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

## 🧠 Diary(Detail)
### Trial(Jan 25)
- reduce batch_size : 64 ->32
- add Data Augumentation(masking)
- increase epochs(10 -> 50)
- chagne last layer of model(output is 1000 -> 10)
- add scheduler(warmup + main)
- add label smoothing
### result
- improve the accuracy(76.8%)
<img width="1589" height="490" alt="image" src="https://github.com/user-attachments/assets/378197d1-426e-4964-8637-b8fb7a0078a8" />

### problem(low accuracy)
- overfitting(less training-set, but resnet18 is relatively large model, which memorize noise and small detail)
- from 25 epoch, that trend can be detected!
### Analysis
- it requires more data augumentation(mixup or cutmix)
- each class has exactly same amount of data(no need focal loss) 
- reduce model-spec(resnet10,8)
- resnet dosen't have dropout
- Batch Normalizaiton vs Group Normalization???

## 🧠 Diary(Detail)
### Trial(Jan 26)
- increase batch_size : 64(32 batch norm is a little bit unstable)
- add Data Augumentation(Cutmix/Mixup)
- increase epochs(50->100)
- custom resnet model(add dropouts, reduce scale of parameter overall)
- adjust label smoothing(0.1->0.05)
- reduce learning rate( 1e-4 -> 1e-3)
- data augumentation for training was also applied to validation dataset!!!!!!!!!
### result
- improve the accuracy(74%)
<img width="1587" height="490" alt="image" src="https://github.com/user-attachments/assets/3bbb3346-d43e-42b7-9ffb-32031d78192f" />


### problem(low accuracy)
- not overfitting, but there's nothing to learn, which means both loss can't decrease and acc can't improve
### Analysis
- need to adjust a model
- hyper parmeter setting
- too much augumentation!


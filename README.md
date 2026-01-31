# 🛠 Consequence(Summary) 🏆:
## Goal : 
reach to best accruacy with balanced and less data sets
## Condition
- training data : 5k,3x96x96, class:10(balanced dataset)
- test data : 8k 3x96x96, class:10(balanced dataset)
- no pretrained model
- no change number of seed(0)
## Rank
- 2nd(89.4%)
<img width="529" height="49" alt="image" src="https://github.com/user-attachments/assets/ac22459a-e9fd-47a3-af56-942a4f1d0e14" />

## Used Algorithm
- model : Custom RestNet + SE Block + Drop path : 64(2)-128(2)-256(2)-512(2) Blocks
- optimizer : Warmup LRscheduling(Linear) + main LRscheduling(CosineAnnealing)- AdamW 
- Criterion : SoftTargetCrossEntorpy(for Mixup/Cutmix) + CrossEntropy + dynamic label-smoothing
- Data-Preprocessing : RandAugment,HorizontalFlip,Mixup/Cutmix
- Technique : TTA + EMA

## Result(92% unofficially)
<img width="1590" height="490" alt="image" src="https://github.com/user-attachments/assets/0fea8373-5afb-496a-87b1-e4ad7429d44e" />
<img width="1934" height="790" alt="image" src="https://github.com/user-attachments/assets/ed13b827-afb7-4e47-9400-81a9986a8ce6" />






# My Journey

## 🛠 Algorithms Used(& Trial):
### optimization
- AdamW -> SGD Momentum -> SWOT -> SGD
- Earlystopping
- warmup scheduler(Linear) 
- main scheudler(ConsineAnnealingLR)
- Dynamic label smoothing scheudling
### config
- epoch : 10 -> 50 -> 100->120->200
- batchsize : 128->64->32->64
### DataAugumentation
- RandomHorizontalFlip
- RandomAugment
- RandomRotation ❌
- ColorJitter ❌
- RandomErasing ❌
- MixUp / CutMix
### TEST
- TTA 2 data augumentation(6 is too much)
### Model(Resnet18)
- custom Resnet(same as 2blocks but different scale : 32-64-128-256, add dropout)->64(3)-128-256
- > 64-128-256-512(each of them has two blocks)
-  fine-tuning layer : 7x7 kernel -> 3x3 kernel ❌ (to heavy 11M parameters)
- last layer : output 1000 -> 10 ❌ 
- label smoothing 
- Stochastic Depth
- SE Block
- K-Fold❌

## 📅 Key milestone(summary)
- **Jan 23:** running basic code & analysis principal problems that contribute to low performance
- **Jan 24:** change SGD to AdamW,add Data Augumentation, increase epochs, chagne first layer of model
- **Jan 25:** add Earlystopping, scheuduler(warmup + main), masking(data pre-prossing), reduce batch-size, increas epoch
- **Jan 26:** add cutmix/mixup, change to custom resnet, increase batch size, no improvement of accuracy
- **Jan 27:** add Random Data augumentation, change the block of models
- **Jan 28:** remove SEblock, remove Masking, add SWOT!!(quite effective), convert model layer
- **Jan 29:** SGD, dynamic label smoothing, reduce vailidation set(10%->8%)




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


## 🧠 Diary(Detail)
### Trial(Jan 27)
- add Random Augumentation
- increase the size of channel in first layer to minize loss of features at the beginning
### result
- improve the accuracy(86%)
<img width="1589" height="490" alt="image" src="https://github.com/user-attachments/assets/601cc8be-6447-4bee-a636-b8e67fb13697" />

### problem(low accuracy)
- changing model structure shows better performance, need to think about a way to improve more....
- most of data augumentation are used
### Analysis
- K-Fold : model is quite stable, so using it might imprvoe 2~3 % of them to vote decision of answers
- SE Block : Squeeze and Excitation, which plays a similar role with attention to capture more detail of features 
- Stochastic Detph : similar to Dropout, but it skips randomly some blocks

### problem(low accuracy)
- not overfitting, but there's nothing to learn, which means both loss can't decrease and acc can't improve
### Analysis
- need to adjust a model
- hyper parmeter setting
- too much augumentation!


## 🧠 Diary(Detail)
### Trial(Jan 28)
- remove K-fold
- remove SE Block
- change optimizer to SWOT(AdamW + SGD momentum)

### result
- improve the accuracy(86%)
<img width="1589" height="490" alt="image" src="https://github.com/user-attachments/assets/601cc8be-6447-4bee-a636-b8e67fb13697" />

### problem(low accuracy)
- SGD is quite sensitive to learning rate and very slow to converge, while Adam is less accurate but guarantees fast convergence) 
- K-fold : it takes too long time to train because it needs to train xN times, also if it has less K, validation proportion is quite high, which means it has less training data set per model
- SE block : I would expect to capture more small detail. but it deteriotate performance( my guess is data augumentation and SE don't have good relation)
- Mixup/cutmix already make label vague, so using label_smoothing makes it worse 
### Analysis
- K-Fold :after making model very very stable, it recommends,otherwise it arouses negative effect
- SE Block : it tries to capture too much detail for small model and less dataset, side-effect
- scale of Data augumentation(Masking, augument, Mixup/Cutmix) need to be adjusted
- Balance between overfitting and underfitting is quite tricky!


### Trial(Jan 29)
- change it to SGD
- split label smoothing more strategically
- final training
- TTA
- reduce validation set(10%->8%)

### result
- improve the accuracy(89%)
<img width="1590" height="490" alt="0130" src="https://github.com/user-attachments/assets/87f02338-7f03-411b-b954-20a3a99fa1e9" />


### problem(maximum performance to this model)
- SGD takes a much longer time to converge than AdamW,but it is still powerful
- as model doesn't show overfitting trend, but literally there is nothing to learn anymore, both training and validation loss get saturated, so when I try to reach that point, I alleviate label-smooting, so it looks like squeezing accuracy a little bit more like 1.5%
- Final training : K-fold takes too much time, but instead of that I use retrain all of dataset(includng validation set), it gives 0.4% augument of accuracy(**but it looks like gambling beacuse adjusting learning rate is quite tricky and we can't know which one is the best model without validation set**
- I try to add one more block at last step hoping it helps learning more, but it doesn't help at all(increasing width seems to be better, but I will pass it to next expriement(less time left)
- 8% arise unstable vibration, but label scheudling and dynamic label-smoothing help converging, but I also try it with 5%, it doesn't give trustable result at all
### Analysis
- As this model, it looks like reaching the maximum performance, but still one day left I will try to experiment another way

### Trial(Jan 30) - experiment
- AdamW
- EMA
- reduce label-smoothing
- back to validation set(10%)
  
### result
- improve the accuracy(92%)
<img width="1590" height="490" alt="image" src="https://github.com/user-attachments/assets/82364cc7-7fce-4a56-a898-e3834fd26740" />
<img width="1934" height="790" alt="image" src="https://github.com/user-attachments/assets/3323a60b-9c17-49ce-a04f-a19914a956b0" />

### problem(maximum performance to this model)
- SGD is very sensitive to adjust learning schedule and slow convergence, so I change it to AdamW
- increase validation-set(5%->10%), which is more stable and less variation
- increase Mixup-cutimix step(50 epochs -> 100 epochs) hoping they can learn more different pattern
- EMA guarantee stable learning with higher accuracy
- when I analysis confusion Matrix,  class 1, 3, 5 has less accuracy(these classes are car, lions, horses, but some pictures are unclear,which has behind view, put them with other objet, less distinctable from backround)
 


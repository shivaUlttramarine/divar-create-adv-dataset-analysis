# Analyzing a "Divar" dataset about "creating new Addvertisment"
Divar is a service like E-bay. 
Here I am working on a dataset of users, who are willing to "Create a New Advertisment".
Dataset includes a seriesof logs, which shows each action of a user, on "Create a new Add" pannel.
for example which page he is in(page_number)? and which of the form-fields are raising an Error or what the action is (action)?

I am trying to 
1. Clean the dataset
2. Identify sequence of logs belong to a use
3. Create a Fact of User-actions
4. Creating KPIs to summarize user-status and user actions.
       For instance 
       page_sum    :  sum of all the pages, user had visited. ex: visiting  2->3->2->3  will result  in 10. This field is used as a code.
                   while,there are just some few top templates which will be revealed later
                   5: visiting 2 ->3
                   8: 2->3->2->1   
                   8: 2->3->2->3  
                   0: visting page 1 and simply leaving the process

        page_cnt    :  How many pages has the user visited in total?

        field_err_cnt: How many Field Error does user face in his/her session or chain?
        Net_err_cnt :  How many Network Error does user face in his/her session or chain?

        EF_xxxxx    :  How many Field Error of type xxxxx did user face? 
        Conversion_rate
        conversion rate  based on "Edit" and "New" use cases:



5. I label most realized patterns( Classic Wizzard ,    Back and Forth ,    to zero), and try to understand what they do they mean.
    
    
4. Analyzing User. Asking and answering some questions, like:
        Q: does NetworkErro effect result?
        A:   p-value = 1  shows that there is not significance interaction between "Net_err_cnt"  and "result"

        Q: Does network Error Effect visited pages?
        A:   p-value = 1  shows that there is not significance interaction between "Net_err_cnt"  and "page_sum"
        A: Errors does not preventusers from going through the process. users visiting more pages, experiencing even more Net Error.
        * lets Just remember that majority of people are stoping at page 1

        Q: why people do not go furthur from page 1 or 2?

        Q: How does  page_cnt effect the result?.A: visting less than 1 page, can result in failur
            A: visting less than 1 page, can result in failur

        Q:getting to know this three groups statistically:
           is_classic_wizzard_user        57497 / (57497+199)   = 99.6 % sucessfull 
           is_back_Forth_user_user        7960  / (1701 +7960 ) = 82.3 % sucessfull  ==>  when page_cnt = 4 then success  -   when page_cnt=5 then fialur
           is_to_zero                     90724 / (90724+171)   = 99.8 % failur
                when the visit is like:        1 ->                           the chance of being succefull is like:1 -( (90724 +171 )   /( 171+ 90724 + 674 + 432 +7960+ 1701 + 57497 + 199) ) = 0.4296176%

                when he reachs page 2 and 3:   1 -> 2 -> 3 ->                 it will be so probable to success   (57497 + 7960+ 1701) /( 674 + 432 +7960+ 1701 + 57497 + 199)  = 98% chance to get succusfull

                if he comes back to 2:         1 -> 2 -> 3  ->  2 ...>1            there is      1701 / (1701+7960)= 17% chance that he chooses 1 in next step  and the process finishes undon

                if he comes back to 2:         1 -> 2 -> 3  ->  2 ...>3            there is      7960 / (1701+7960)= 82% chance that he chooses 3 in next finishs the process succesfully      


        Q: What are the most effictive Errors? Preventing user to procced? 
        #  Below fields can effect user, and stop him by page  1 or 2
        #  FE_location
        #  FE_category
        #  FE_description
        #  FE_title


        Q : is there any relationship between Field Error and usage_pattern?
        A :  It does not significantly show any causuality relationship. 
             in segment#2 (Classical_wizzard_users), which are 99% successful, we can see most field Errors.
             They can not be taken as any obstacle.


6.Trying logetic regression to find out which factors play a role in finishing the job successfully
        # 1. how does factors effect result?
        # 2. how does factors obstacle user in page 0?
        # 3. how does factors obstacle user in 1-2-3-2 to comebacke to page 1?

        # how does factors effect result?



7. How does field Errors effect user to be "is_to_zero"  segment?

A. by testing filled and its intraction on page_cnt, we highlight that, below filed, negatively effect user,
   put him/her in "to_zero" serment, 
   and stops him/her from going furthur in the wizzard
   
    # page_cnt2:FE_description1    8.041e+00  5.985e-01  13.435  < 2e-16 ***
    # page_cnt2:FE_title1          8.706e+00  7.209e-01  12.076  < 2e-16 ***
    
    
        
    
    
#    ----------------------------    Actionable Results --------------------------------
    so:
1. what makes people stock  on first pages and do no proceed to next page?
   sometimes they are just trying "Add advertise" butt
   or
   sometimes, based on logestic regression, there are some fileds, that significiantly, increase the chance of being in "is_to_zero" segment.
   Description  +  title
   errors on this field increase the chance of being in "is_to_zero" segment, therefor decrease the chance of visiting other pages,
   that is capstone of ending unsuccessfully.
   on the other hand, there are some fileds on age 3, which attract usere to click on finish and submite the advertisment.
   we need to make user visit that page faster. As I tried this dunction on Divar App, I found out page 3, includes lots of "Selection Components".
   They are easy to fill, and attractive for user. But Title and Descrption textboxs, are hard  to fill. It's hard for android user to fill it.
   I guess users give up, when they see this 2 big textboxes and they miss fantastic multi-selection components of page3.
   
   
   suggestion:
   what if you put Description and Title Textboxs on page 3, and show users attractive multi-selection components first?
   they are easy to fill out, and when user reach Desciption and Title and very last page, he/she would more likely fill the boxs, cause that's the last       step.He wont give up whole process now, while he has already filled a lot. that would be a wast to quite at page 3.
   
2.What makes people reach page 3, come backe to page 2, but never get back to the wizzard fot page 3 agian?
   I did not have enough time to answer this question
   but there are 20% of  "is_back_Forth_user_user" segment, that we lose on this regard.





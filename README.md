#Data Structures(CS2302)
#Lester Ibarra
#Sentiment Analysis
#Aguirre, Diego
#Nath, Anindita
import nltk
from nltk.sentiment.vader import SentimentIntensityAnalyzer
import praw
reddit = praw.Reddit(client_id='HjIibYz3lhgCJg',
                     client_secret='SBMs3bHf_jvEd18IB96Z8kgqVI4',
                     user_agent= 'Llibarra2'
                     )#This allows access to the subreddit
nltk.download('vader_lexicon')
sid = SentimentIntensityAnalyzer()

def get_text_negative_proba(text):
    return sid.polarity_scores(text)['neg']#provides a score between 1 and 0, closests to 1 meaning a negative comment

def get_text_neutral_proba(text):
    return sid.polarity_scores(text)['neu']#provides a score between 1 and 0, closests to 1 meaning a neutral comment

def get_text_positive_proba(text):
    return sid.polarity_scores(text)['pos']#provides a score between 1 and 0, closests to 1 meaning a positive comment

def get_submission_comments(url):#receives url of subreddit and returns all comments including replies as well
    submission = reddit.submission(url=url)
    submission.comments.replace_more(limit=None)

    return submission.comments

def comment_processor(text, n):#receives array of all comments and replies as well as an integer with starting value 0
    list_neg,list_pos,list_neu = [],[],[] 
    if n==len(text):
        return list_neg,list_pos,list_neu#returns three list contaning all possible comments with their respective sensitivity
    comment = text[n].body
    pos = get_text_positive_proba(comment)#gives value for comment being positive
    neg = get_text_negative_proba(comment)#gives value for comment being negative
    neu = get_text_neutral_proba(comment)#gives value for comment being neutral
    if(pos>neg and pos>neu):#compares values received to see dominating sensitivity
        list_pos.append(comment)
    if(neg>pos and neg>neu):
        list_neg.append(comment)
    if(neu>pos and neu>neg):
        list_neu.append(comment)
    n+=1
    comment_processor(text, n)#recursive statement
    
def main():#code was tested with (https://www.reddit.com/r/politics/comments/9gxu84/donald_trump_is_actively_obstructing_justice/ and
            #https://www.reddit.com/r/dogs/comments/9h0ela/fluff_i_realized_my_dog_is_a_solid_protector/)
            #These two subreddits were tested as they involve politics which can be very much contriversial, meaning negative comments would be more precedent
            #and story of a dog, which would most likely contain positive comments
    comments = get_submission_comments('https://www.reddit.com/r/learnprogramming/comments/5w50g5/eli5_what_is_recursion/')
    all_comments_and_replies = comments.list()#able to list all comments and replies from subreddit
    n = 0
    list_neg,list_pos,list_neu = [],[],[] 
    list_neg,list_pos,list_neu = comment_processor(all_comments_and_replies, n)#The returned lists are placed inside new lists

    print("Negative Comments")
    for i in range(len(list_a)):#Prints all negative comments
        print(list_a[i])

    print("Positive Comments")    
    for i in range(len(list_b)):#Prints all positive comments
        print(list_b[i])

    print("Neutral Comments")
    for i in range(len(list_c)):#Prints all neutral comments
        print(list_c[i])
main()


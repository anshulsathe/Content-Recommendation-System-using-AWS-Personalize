
# Content Recommendation System using AWS Personalize

For creating this content recommendation system, we will have to use mainly two services, Amazon S3 and Amazon Personalize. Amazon personalise is used to implement algorithms and use them on datasets to get personalised rankings for a particular user for every listed product. We have used amazon-personalised-ranking algorithm in this implementation and this algorithm gives us scores of the particular item/movie out of 1; 0 being the worst ranking and 1 being the best ranking


## Steps

1. Create a dataset on Amazon Personalise. There will be 3 datasets required for this model, User-Item interaction dataset, Item dataset and User dataset. We are going to select a dataset for movies.

2. While uploading the datasets, we have to upload the datasets in an S3 bucket and enter the endpoint(URI) of the item in the bucket in Personalize. We have to ensure that in the bucket policy the S3 bucket is given access to Personalize service. To do that edit the bucket policy as:
```bash
{
    "Version": "2012-10-17",
    "Id": "PersonalizeS3BucketAccessPolicy",
    "Statement": [
        {
            "Sid": "PersonalizeS3BucketAccessPolicy",
            "Effect": "Allow",
            "Principal": {
                "Service": "personalize.amazonaws.com"
            },
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::name-of-bucket",
                "arn:aws:s3:::name-of-bucket/*"
            ]
        }
    ]
}
```
3. Before uploading the dataset we have to ensure that the schema of the dataset is matching the custom input schema that we give in aws personalise. For the three datasets we have given the schemas as follows:
- User-Item Interactions
```bash
{
	"type": "record",
	"name": "Interactions",
	"namespace": "com.amazonaws.personalize.schema",
	"fields": [
		{
			"name": "USER_ID",
			"type": "string"
		},
		{
			"name": "ITEM_ID",
			"type": "string"
		},
		{
			"name": "RATING",
			"type": "string"
		},
		{
			"name": "TIMESTAMP",
			"type": "long"
		}
	],
	"version": "1.0"
}
```
- User dataset:
```bash
{
	"type": "record",
	"name": "Users",
	"namespace": "com.amazonaws.personalize.schema",
	"fields": [
		{
			"name": "USER_ID",
			"type": "string"
		},
		{
			"name": "MOVIE_ID",
			"type": "int"
		},
		{
			"name": "TAG",
			"type": "string"
		},
		{
			"name": "TIMESTAMP",
			"type": "string"
		}
	],
	"version": "1.0"
}
```

- Item Dataset:
```bash
{
	"type": "record",
	"name": "Items",
	"namespace": "com.amazonaws.personalize.schema",
	"fields": [
		{
			"name": "ITEM_ID",
			"type": "string"
		},
		{
			"name": "TITLE",
			"type": [
				"null",
				"string"
			],
			"categorical": true
		},
		{
			"name": "GENRE",
			"type": [
				"null",
				"string"
			],
			"categorical": true
		}
	],
	"version": "1.0"
}
```
#### After about half an hour, we will have all these datasets active in personalize.

4. Now it is time to create a solution. These are algorithms which will give us the outputs containing personalized rankings of any product as per any user. For this model we have used Amazon-Personalized-Ranking algorithm which is also known as a recipe. This algorithm generates personalized rankings and recommendations for users based on their preferences and historical behaviour. It uses machine learning to analyse data and factors like user behaviour and item attributes to provide accurate and tailored recommendations.
It can take anywhere around 2 hours to 48 hours for this solution to train according to our input dataset.

5. This solution is now run using a campaign. A campaign has 2 input parameters inside of AWS Personali itself
The first parameter contains USER_ID where and the second parameter contains ITEM_ID which can take multiple inputs separated by commas. On clicking get personalised rankings, we will have the items ranked as per the user where 1 is the best ranking and 0 is the worst ranking.

- Therefore by just integrating these two simple services, we have successfully created a back-end for a Content recommendation system which will allow us to get personalised rankings and tell us that a user is more likely to like which items.








## Documentation

[Link to the Dataset](https://drive.google.com/drive/folders/1zpe4dqlkvIIsFqqjkM-ZpkKV80N4JInO?usp=sharing)

Here the three datasets corelate to:
- Movies (Item dataset)
- Ratings (User-Item interaction dataset)
- User (User dataset)

## Features
- _User profiles_: User profiles store information about a user's past behavior and preferences. This information is used to generate recommendations.
- _Content profiles_: Content profiles store information about the content that is being recommended. This information can include the content's title, genre, rating, and other metadata.
- _Recommendation algorithms_: Recommendation algorithms are used to generate recommendations based on user profiles and content profiles. There are many different recommendation algorithms, each with its own strengths and weaknesses.



## Algorithms

There are many different algorithms that can be used by content recommendation systems. Some of the most common algorithms include:

- _Cosine similarity_: Cosine similarity is a measure of similarity between two vectors. It is often used to measure the similarity between user profiles and content profiles.
- _Nearest neighbors_: Nearest neighbors is a method for finding similar items. It works by finding the items that are most similar to a given item.
- _Bayesian networks_: Bayesian networks are a type of probabilistic graphical model. They can be used to model the relationships between user profiles and content profiles.

## Conclusion

- Overall Effectiveness of Content Recommendation Systems

Content recommendation systems have been shown to be effective in a variety of applications. For example, a study by Netflix found that their recommendation system increased customer satisfaction by 10%.
However, content recommendation systems are not perfect. They can sometimes make recommendations that are not relevant to the user. Additionally, they can be difficult to tune, and they can be computationally expensive.

- Example of a Content Recommendation System

One example of a content recommendation system is the Netflix recommendation system. The Netflix recommendation system is based on a variety of factors, including the user's viewing history, ratings, and the ratings of other users. The Netflix recommendation system has been shown to be very effective, and it is responsible for a significant portion of Netflix's revenue.

- Conclusion

Content recommendation systems are a powerful tool for providing users with personalized content. They are used in a variety of applications, and they have been shown to be effective in increasing customer satisfaction. However, content recommendation systems are not perfect, and they can sometimes make recommendations that are not relevant to the user.


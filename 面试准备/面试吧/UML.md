``` mermaid
classDiagram

class BasicObject {
    - Long id
    - Date createTime
    - Date updateTime
    - Boolean isDeleted
}

class User{
    - String uid
    - String userName
    - Date birthDate
    - String email
    - String phoneNumber
    - String openId
    - String githubAccount
    - String githubToken
    - String password
    - String IDNumber
}
BasicObject <|-- User

class SocialInformation{
    - String uid
    - String icon
    - Boolean isAuthorizedMatch
    - List<Long> positionIntentions
    - List<Long> workExperience
    - String selfIntroduction
    - int interviewTickets
    - HashMap checkInRecords
    - List<Long> mySparks
    - List<Long> favoriteSparks
    - List<Long> myQUestions
    - List<Long> favoruteQuestions
}
BasicObject <|-- SocialInformation

class CompanyInfo {
    - String companyName
    - String officeAddress
    - String contactNumber
    - String emailAddress
    - String website
    - Date establishmentDate
    - Double registeredCapital
    - List<String> businessScope
    - String companyIntroduction
}
BasicObject <|-- CompanyInfo

class PositionInfo{
    - String positionName
    - String positionType
}
BasicObject <|-- PositionInfo
SocialInformation "1" --> "1..n" PositionInfo

class WorkExperience{
    - Long companyId
    - String cmpanyName
    - Long positionId
    - String positionName
    - String positionType
    - String workConent
    - Date entryTime
    - Date resignationTime
    - Boolean isVerified
}
BasicObject <|-- WorkExperience
WorkExperience "1" --> "1" CompanyInfo
WorkExperience "1" --> "1" PositionInfo
SocialInformation "1" --> "1..n" WorkExperience

class BasicSocialEntity {
    - List<Long> comments
    - List<String> tags
    - double likes
    - double dislikes
    - double favorites
    - duoble shares
    - ShareModeEnum shareMode
    - String shareLink
}
BasicObject <|-- BasicSocialEntity

class Spark {
    - String authorId
    - String title
    - String content
    - List<Image> images
}
BasicSocialEntity <|-- Spark
SocialInformation "1" --> "1..n" Spark

class Image {
    - String ossUrl
    - String fileName
    - String hintText
}
BasicObject <|-- Image
Spark "1" --> "1..n" Image

class Comment {
    - String authorId
    - Long targetId
    - CommentTargetEnum targetType
    - String content
    - double likes
    - double dislikes 
}
BasicObject <|-- Comment
Spark "1" --> "1..n" Comment
Comment "1" --> "1" Comment

class PartnerPlan {
    - String planName
    - String description
    - List<Long> tasks
    - Date startDate
    - Date expirationDate
    - List<Json> milestones
    - Boolean isCompleted
    - List<Long> releationInterviews
    - String authorId
    - String updateUserId
}
BasicSocialEntity <|-- PartnerPlan
PartnerPlan "1" --> "1..n" PartnerTask
PartnerPlan "1" --> "1..n" Comment
PartnerPlan "1" --> "1..n" InterviewRecord

class PartnerTask {
    - String taskName
    - String taskContent
    - Date startDate
    - Date expirationDate
    - List<Long> relationSparks
    - List<Long> relationQuestions
    - Boolean isCompleted
    - String authorId
    - String updateUserId
}
BasicObject <|-- PartnerTask
PartnerTask "1" --> "1..n" Spark
PartnerTask "1" --> "1..n" Question

class UserPlanRelation {
    - String uid
    - Long planId
    - Enum permission
}
BasicObject <|-- UserPlanRelation
SocialInformation "1" --> "1" UserPlanRelation
PartnerPlan "1" --> "1" UserPlanRelation


class Question {
    - String question
    - String answer
    - String authorId
}
BasicSocialEntity <|-- Question

class InterviewRecord {
    
}


```
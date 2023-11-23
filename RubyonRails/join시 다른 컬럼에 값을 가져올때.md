# todo: 원리에 대해 링크혹은 설명 추가 필요

```ruby
  has_many :comments
  belongs_to :user
  # user의 입장에서 comments와 join하고 where 조건을 메인인 user가 아닌 comment의 컬럼을 조회할 때는
  # 컬럼을 {} 로 감싸줘야 정상적으로 가져온다.


  # 잘못된 쿼리
  joins("INNER JOIN comment on comment.id = user.commentable_id").where(comment: other: template)
  # 정상쿼리
  joins("INNER JOIN comment on comment.id = user.commentable_id").where(comment: {other: template})
```

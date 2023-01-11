# django Tip

# django 모델의 정보를 SQL에 반영시키고 싶지 않다면

class Meta:  
  abstract = True
  
를 사용


``` python
class CommonModel(models.Model):
    """Common Model Definition"""
    
    created_at = models.DateTimeField(auto_now_add=True,)
    update = models.DateTimeField(auto_now=True,)
   
    class Meta:
        abstract = True
```

# app의 model에서 ForeignKey를 선언할 때 항상 on_delete를 설정
``` python
user = models.ForeignKey(
    "users.User",
    on_delete = models.CASCADE
)
```
 - on_delete : ForeignKey 주체가 삭제될 시 설정
  - CASCADE : 해당 정보도 삭제
  - SET_NULL : NULL값으로 처리. 이를 위해선 `null = True`  , `blank = True` 함께 설정
   ``` python
    user = models.ForeignKey(
        "users.User",
        null  = True,
        blank = True,
        on_delete = models.SET_NULL,
    )
    ```

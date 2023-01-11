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

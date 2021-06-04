# api1



import email
from os import name

from myapp import models


class Candidate(models.Model):
 name = models.CharField(max_length=100)
 email = models.EmailField(max_length=100)


class TestScore(models.Model):
  candidate = models.ForeignKey(candidate)
  test_score1 = models.PositiveNumberField(default=0)
  test_score2 = models.PositiveNumberField(default=0)
  test_score3 = models.PositiveNumberField(default=0)
  total_score = models.PositiveNumberField(default=0)

 def save(*args, **kwargs):
  self.total_score = self.test_score1+self.test_score2+self.test_score3
  return super(TestScore, self).save(*args, **kwargs)



def get_highest_score_candidate(request):
 
  candidate = TestScore.objects.all().order_by("-total_score")[0].candidate
 

 data = {name: candidate.name, email: candidate.email, score: candidate.total_score}

return JsonResponse(data)

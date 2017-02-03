
```
from django.core.exceptions import ValidationError
from authnorbl.sce_redis import SceRedis
from django.db.models import signals
from django.db.models.signals import post_delete
from django.dispatch import receiver

@receiver(post_delete, sender=NoRBL)
def delete_comment_after(sender, instance, **kwargs):
    ipkey = "{0}_{1}".format(settings.NO_RBL_REDIS_PREFIX, instance.target)
    myredis = SceRedis.instance().redis_master
    myredis.delete(ipkey)
    # 更新历史记录表
    record = NoRBL_Record()
    record.target = instance.target
    record.reason = instance.reason
    record.end_time = instance.end_time
    record.action = "delete"
    record.save()
```

```
actions = ['delete_selected']

def delete_selected(self, request, obj):
    redisclient = RedisClient(using=True)
    for o in obj.all():
        utils.admin_utils.clear_activity_item_redisinfo(redisclient, o.activity_id, o.id)
        o.delete()

delete_selected.short_description = 'delete selected items'
```

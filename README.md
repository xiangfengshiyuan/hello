# hello
this is my first text
queue* q_new(void)
{
   return (queue *)MALLOC(sizeof(queue));
}

int q_release(queue *q)
{
   if (NULL != q)
      MFREE((void *)q);

   return 0;
}

int q_init(queue *q)
{
   list_init(&q->que);
   list_init(&q->free);

   return 0;
}

int q_uninit(queue *q)
{
   list_node *item;
   if(NULL==q)				//added by wlyu2
   {
	   return 0;
   }
   while ((item = list_pop_front(&q->free)) != NULL )
      MFREE((void *)item);

   return 0;
}

void* q_pop(queue *q)
{
   list_node *item = list_pop_front(&q->que);
   void *ret = NULL;

   if (NULL != item) {
      ret = item->val;
      list_push_back(&q->free, item); 
   }

   return ret;
}


int q_push(queue *q, void *item)
{
   list_node *free = list_pop_front(&q->free);

   if (NULL == free)
      free = (list_node *)MALLOC(sizeof(list_node));
   if (NULL == free)
      return -2;

   free->val = item;
   list_push_back(&q->que, free);
   
   return 0;
}

int q_size(queue *q)
{
   return list_size(&q->que);
}

int q_empty(queue *q)
{
   return list_empty(&q->que);
}

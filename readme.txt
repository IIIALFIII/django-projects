python manage.py shell

Создайте двух пользователей:

from django.contrib.auth.models import User

user1 = User.objects.create_user('username1')
user2 = User.objects.create_user('username2')

Создайте два объекта модели Author, связанные с пользователями:

from myapp.models import Author

author1 = Author.objects.create(user=user1)
author2 = Author.objects.create(user=user2)

Добавьте 4 категории в модель Category:

from myapp.models import Category

category1 = Category.objects.create(name='Category 1')
category2 = Category.objects.create(name='Category 2')
category3 = Category.objects.create(name='Category 3')
category4 = Category.objects.create(name='Category 4')

Добавьте 2 статьи и 1 новость и присвойте им категории:

from myapp.models import Post, News

post1 = Post.objects.create(author=author1, title='Post 1', content='Content 1')
post1.categories.add(category1, category2)

post2 = Post.objects.create(author=author2, title='Post 2', content='Content 2')
post2.categories.add(category3, category4)

news1 = News.objects.create(author=author1, title='News 1', content='Content 3')
news1.categories.add(category1, category3)

Создайте как минимум 4 комментария к разным объектам модели Post:

from myapp.models import Comment

comment1 = Comment.objects.create(post=post1, author=user1, content='Comment 1')
comment2 = Comment.objects.create(post=post2, author=user2, content='Comment 2')
comment3 = Comment.objects.create(post=post1, author=user2, content='Comment 3')
comment4 = Comment.objects.create(post=post2, author=user1, content='Comment 4')

Примените функции like() и dislike() к статьям/новостям и комментариям, чтобы скорректировать их рейтинги:

post1.like()
post1.dislike()
post2.like()
comment1.like()
comment2.dislike()
comment3.like()
comment4.dislike()

Обновите рейтинги пользователей:

author1.update_rating()
author2.update_rating()

Выведите username и рейтинг лучшего пользователя:

best_author = Author.objects.order_by('-rating').first()
print(best_author.user.username, best_author.rating)

Выведите дату добавления, username автора, рейтинг, заголовок и превью лучшей статьи:

best_post = Post.objects.order_by('-rating').first()
print(best_post.date_added, best_post.author.user.username, best_post.rating, best_post.title, best_post.preview())

Выведите все комментарии (дата, пользователь, рейтинг, текст) к этой статье:

comments = Comment.objects.filter(post=best_post)
for comment in comments:
    print(comment.date_added, comment.author.username, comment.rating, comment.content)
## GitLab 운영관련 이슈 정리

### root password reset 

```ruby
gitlab-rails console -e production
user = User.where(id: 1).first
user.password = 'secret_pass'
user.password_confirmation = 'secret_pass'
user.save!
```

- https://docs.gitlab.com/12.10/ee/security/reset_root_password.html


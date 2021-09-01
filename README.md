# NoMethodError-undefined-method-enqueue-ActiveJob
> Using Rails `4.1.6`, Ruby `2.3.1`.
```ruby
# my_model.rb
def send_notification
  ...
  MyNotificationJob.enqueue({key1: val1, key2: val2})
  ...
end

# my_models_controller.rb
def publish
  ...
  my_model_instance.send_notification
  ...
end
```
When I am trying to write unit test `send_notification`
```ruby
# spec/rails_helper.rb
 RSpec.configure do |config|
   config.include ActiveJob::TestHelper
 end

# spec/my_models_controller_spec.rb
 require 'rails_helper'

 RSpec.describe MyModelsController, type: :controller do
   it 'enqueues the job' do
     get :publish
   end
```
I am getting following error
```ruby
NoMethodError: undefined method `enqueue' for MyNotificationJob:Class
```
After looking into the [documentation](https://api.rubyonrails.org/classes/ActiveJob/Enqueuing.html#method-i-enqueue), I realised `enqueue` must be called upon an instance. But as you can see above I am calling it on `MyNotificationJob` class and it works fine in production.

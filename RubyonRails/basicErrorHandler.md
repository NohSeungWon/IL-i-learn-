```ruby
  #application.rb
    # default는 아래와 같이 되어있다.
    # {"ActiveRecord::RecordNotFound"=>:not_found, "ActiveRecord::StaleObjectError"=>:conflict, "ActiveRecord::RecordInvalid"=>:unprocessable_entity, "ActiveRecord::RecordNotSaved"=>:unprocessable_entity}
    # custom
    config.action_dispatch.rescue_responses.merge!({
      "ActionController::RoutingError" => :not_found,
      "ActionController::UnknownFormat" => :bad_request,
      "ActionController::InvalidAuthenticityToken" => :unprocessable_entity,
      "ActionController::InvalidCrossOriginRequest" => :unprocessable_entity,
      "ActionController::BadRequest" => :bad_request,
      "ActionController::ParameterMissing" => :bad_request,
      "ActionDispatch::Http::Parameters::ParseError" => :bad_request,
      "ActiveRecord::RecordNotUnique" => :unprocessable_entity,
      "CanCan::AccessDenied" => :forbidden,
      "ApplicationException::NotFound" => :not_found
    })
```

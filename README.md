== README

=== Getting Nested Forms to Work

====== app/controller/surveys_controller.rb
  def new
    @survey = Survey.new
    3.times do
      question = @survey.questions.build 
      4.times { question.answers.build }
    end
  end
  def survey_params
    params.require(:survey).permit(
    :name,
    questions_attributes: [:id, :survey_id, :content, :_destroy,
     answers_attributes: [:id, :question_id, :content, :_destroy]])
  end  

=== Getting Remove to Work

====== app/views/surveys/_answer_fields.html.erb
<div class="field">
    <%= f.label :content, "Answers" %>
    <%= f.text_field :content %>
    <%= f.hidden_field :_destroy %>
    <%= link_to "remove", "#", class: "remove_fields" %>
</div>


====== app/assets/javascript/survey.js.coffee
ready = ->
  $('form').on 'click', '.remove_fields', (event) ->
    $(this).prev('input[type=hidden]').val('1')
    $(this).closest('fieldset').hide()
    event.preventDefault()

  $('form').on 'click', '.add_fields', (event) ->
    time = new Date().getTime()
    regexp = new RegExp($(this).data('id'), 'g')
    $(this).before($(this).data('fields').replace(regexp, time))
    event.preventDefault()

$(document).ready(ready)
$(document).on('page:load', ready)


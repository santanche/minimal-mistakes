# Digital Content Components

## Playground

Learn and try to instantiate and customize Digital Content Components (DCCs) at [DCC Playground](http://datasci4health.github.io/harena-space/src/adonisjs/public/dccs/playground/).

# Syntax and Examples

## Trigger DCC (`<dcc-trigger>`)

A visual element that triggers an action. Its standard shape is a button, but it can be also an image or an element customized by the author.

### Syntax

~~~html
<dcc-trigger id="id"
             label="label"
             image="image"
             action="action"
             value="value">
</dcc-trigger>
~~~

* `id` - unique id of the trigger;
* `label`:
  * textual button - textual label showed in the button;
  * image trigger - the title of the image;
* `image` (optional) - when the trigger is an image, it is the path of the image file;
* `action` (optional) - the topic of the message sent by the trigger to activate an action; when the action is not specified, the topic is built from the label ("trigger/<label>/clicked");
* `value` (optional) - the message body the accompanies the topic.

### Examples

Textual button trigger that sends the following message when clicked:
* topic - `button/on/clicked`
* message body - `"message to you"`

~~~html
<dcc-trigger label="On"
             action="button/on/clicked"
             value="message to you">
</dcc-trigger>
~~~

Image trigger with title `check` and whose image is located in `icons/icon-check.svg`. Since the image occupies all available area, a `<div>` surrounding it delimits the size to `100px`.

When clicked, the trigger will send a message with the topic: `trigger/Check/clicked`.

~~~html
<div style="width: 100px">
   <dcc-trigger label="Check"
                image="icons/icon-check.svg">
   </dcc-trigger>
</div>
~~~

## Lively Talk DCC (`<dcc-lively-talk>`)

An animated image that also displays a text inside a ballon. Usually adopted for animated dialogs.

### Syntax

~~~html
<dcc-lively-talk duration="duration" 
                 delay="delay"
                 direction="direction"
                 character="character"
                 speech="speech">
</dcc-lively-talk>
~~~

* `duration` - duration of the animation (duration=0 means a static image);
* `delay` - delay before the animation is started;
* `direction` - direction of the animation (`left` (default) or `right`);
* `character` - character that appears in the image;
* `speech` - text of the speech.

When a Lively Talk DCC receives a message, it shows the body of the message as a speech in the ballon.

### Examples

Available characters in the playground: nurse, doctor, and patient.

A static patient showing the speech "Please, help me!"

~~~html
<dcc-lively-talk duration="0"
                 character="patient"
                 speech="Please, help me!">
</dcc-lively-talk>
~~~

An animated nurse that enters in 2 seconds and shows the speech "Doctor, please you have to evaluate a man!"

~~~html
<dcc-lively-talk duration="2s"
                 character="nurse"
                 speech="Doctor, please you have to evaluate a man!">
</dcc-lively-talk>
~~~

An animated doctor that enters in 2 seconds after waiting 2 seconds and shows the speech "Ok, I'm on my way." The doctor's animation goes in the right direction.

~~~html
<dcc-lively-talk duration="2s"
                 delay="2s"
                 direction="right"
                 character="doctor"
                 speech="Ok, I'm on my way.">
</dcc-lively-talk>
~~~

### Talks Inside a Dialog

Talks can be grouped inside a `<dcc-lively-dialog>`, which define the parameters of the complete dialog.

~~~html
<dcc-lively-dialog rate="6s" duration="2s">
   <dcc-lively-talk character="nurse"
                    speech="Doctor, please you have to evaluate a man!">
   </dcc-lively-talk>
   <dcc-lively-talk character="doctor"
                    speech="Ok, I'm on my way.">
   </dcc-lively-talk>
</dcc-lively-dialog>
~~~

## Subscribing Messages and Connecting Components (`<subscribe-dcc>`)

A DCC can subscribe to message in such a way that whenever the message appears on the bus, it will receive it.

For each subscribed message a DCC declares a `<subscribe-dcc>` inside its element. With the following syntax:

~~~html
<subscribe-dcc topic="message"></subscribe-dcc>
~~~

* message - specifies the subscribed message

The following example shows the message `I am a doctor.` when the button with the label `Talk` is triggered.

~~~html
<dcc-trigger label="Talk" action="send/message" value="I am a doctor.">
</dcc-trigger>

<dcc-lively-talk id="doctor"
                 duration="0s"
                 character="doctor"
                 speech="Hello, ">
  <subscribe-dcc topic="send/message"></subscribe-dcc>
</dcc-lively-talk>
~~~

The following example shows a character that tells you "Hello *your name*" when you type your name.

~~~html
<dcc-input variable="name">Type your name:</dcc-input>

<dcc-lively-talk id="doctor"
                 duration="0s"
                 character="doctor"
                 speech="Hello ">
  <subscribe-dcc topic="var/name/changed"></subscribe-dcc>
</dcc-lively-talk>
~~~

Or how a character tells you "Your age is *your age*" when you define your age in the slider.

~~~html
<dcc-slider variable="age" index>Select your age:</dcc-slider>

<dcc-lively-talk id="doctor"
                 duration="0s"
                 character="doctor"
                 speech="Your age is  ">
  <subscribe-dcc topic="var/age/changed"></subscribe-dcc>
</dcc-lively-talk>
~~~

To present a multiple-choice input:

For `<dcc-input-option>`:
* `parent` - parent of the option explicitly declared (otherwise will be inferred by the hierarchy);
* `variable` - variable of the option explicitly declared, otherwise will assume the parent's variable (which is the expected scenario);
* `exclusive` - exclusivity of the option explicitly declared, defines if the value will be exclusive (radio button) or not (checkbox), otherwise will assume the parent's exclusivity (which is the expected scenario);


~~~html
<dcc-input-choice variable="role" statement="What is your role?" exclusive>
   <dcc-input-option>patient</dcc-input-option>
   <dcc-input-option>doctor</dcc-input-option>
   <dcc-input-option>nurse</dcc-input-option>
</dcc-input-choice>
~~~

~~~html
<dcc-input-choice variable="role" statement="What is your role?" exclusive>
   <dcc-input-option value="patient">I am a patient</dcc-input-option>
   <dcc-input-option value="doctor">I am a doctor</dcc-input-option>
   <dcc-input-option value="nurse">I am a nurse</dcc-input-option>
</dcc-input-choice>
~~~

~~~html
<dcc-unfold><dcc-unfold-short>I am a patient</dcc-unfold-short></dcc-unfold>
~~~

~~~html
<dcc-input-choice variable="role" exclusive>
   What is your role?
   <dcc-input-option value="patient">I am a patient</dcc-input-option>
   <dcc-input-option value="doctor">I am a doctor</dcc-input-option>
   <dcc-input-option value="nurse">I am a nurse</dcc-input-option>
</dcc-input-choice>
~~~

~~~html
<dcc-input-choice variable="role" statement="What is your role?" exclusive>
   <dcc-input-option value="patient">I am a patient</dcc-input-option>
   <dcc-input-option value="doctor">I am a doctor</dcc-input-option>
   <dcc-input-option value="nurse">I am a nurse</dcc-input-option>
</dcc-input-choice>

<dcc-lively-talk id="doctor"
                 duration="0s"
                 character="doctor"
                 speech="I am a ">
  <subscribe-dcc topic="var/role/changed"></subscribe-dcc>
</dcc-lively-talk>
~~~

~~~html
<dcc-input-choice variable="role" statement="What is your role?" exclusive>
   <dcc-input-option value="patient">I am a patient</dcc-input-option>
   <dcc-input-option value="doctor">I am a doctor</dcc-input-option>
   <dcc-input-option value="nurse">I am a nurse</dcc-input-option>
</dcc-input-choice>

<dcc-unfold message="var/role/changed" value="patient">You must wait here.</dcc-unfold>

<dcc-lively-talk id="doctor"
                 duration="0s"
                 character="doctor"
                 speech="I am a ">
  <subscribe-dcc topic="var/role/changed"></subscribe-dcc>
</dcc-lively-talk>
~~~

This is a multi-entry input presented as a table.

~~~html
<dcc-input-table id="person_list" variable="person" rows="3" schema="name, age, height">
</dcc-input-table>
~~~

## Image DCC (`<dcc-image>`)

* `style` - replicated attribute

~~~html
<dcc-image image="images/mv/mv-front-base.svg" style="width:300px"></dcc-image>
<dcc-image image="images/mv/mv-back-base.svg" style="width:300px"></dcc-image>
<dcc-state id="screen" value="off" rotate>
   <dcc-image role="off" image="images/mv/mv-screen-off.svg"
              style="position:absolute;left:36px;top:28px;width:193px">
   </dcc-image>
   <dcc-group role="start">
      <dcc-image image="images/mv/mv-screen-start.svg"
                 style="position:absolute;left:36px;top:28px;width:193px">
      </dcc-image>
      <dcc-image image="images/mv/mv-ventilation-mode.svg"
                 style="position:absolute;left:48px;top:30px;width:40px">
         <trigger-dcc event="click" target="screen" role="state" value="mode"></trigger-dcc>
      </dcc-image>
   </dcc-group>
   <dcc-image role="mode" image="images/mv/mv-mode-options.svg"
              style="position:absolute;left:36px;top:28px;width:193px">
   </dcc-image>
</dcc-state>
<dcc-state id="power-button" value="on" rotate>
   <dcc-image role="on" image="images/mv/mv-power.svg"
              style="position:absolute;left:190px;top:113px;width:20px">
      <trigger-dcc event="click" target="screen" role="state" value="start"></trigger-dcc>
      <trigger-dcc event="click" target="power-button" role="next"></trigger-dcc>
   </dcc-image>
   <dcc-image role="off" image="images/mv/mv-power-pressed.svg"
              style="position:absolute;left:190px;top:113px;width:20px">
      <trigger-dcc event="click" target="screen" role="state" value="off"></trigger-dcc>
      <trigger-dcc event="click" target="power-button" role="next"></trigger-dcc>
   </dcc-image>
</dcc-state>
~~~

## Trigger (`<trigger-dcc>`)

* `source` - explicitly indicates the source when the `<trigger-dcc>` element is not subordinated to another DCC;
* `event` - a monitored event which triggers the notification;
* `target` - target of the notification;
* `role` - the role of the notification - related to the action that will be taken by the target when receives the notification;
* `publish` - message to be published in the bus;
* `property` - property of the source DCC whose value will be sent to the target;
* `value` - value to be sent to the target (named as "value" property).

## State DCC (`<dcc-state>`)

~~~html
<dcc-state value="first" rotate>
  <dcc-image role="first" image="images/doctor-icon.png"></dcc-image>
  <dcc-image role="second" image="images/nurse-icon.png"></dcc-image>
  <dcc-image role="third" image="images/patient-icon.png"></dcc-image>
  <subscribe-dcc topic="state/next" role="next"></subscribe-dcc>
</dcc-state>

<dcc-trigger label="Next" action="state/next">
</dcc-trigger>
~~~

~~~html
<dcc-state value="first" variable="state" rotate>
  <dcc-image role="first" image="images/doctor-icon.png"></dcc-image>
  <dcc-image role="second" image="images/nurse-icon.png"></dcc-image>
  <dcc-image role="third" image="images/patient-icon.png"></dcc-image>
  <trigger-dcc event="click" role="next"></trigger-dcc>
</dcc-state>
~~~

## Web DCC `<dcc-web>`

~~~html
<dcc-image id="mv-front" image="images/mv/mv-front.svg" style="width:200px"></dcc-image>
<dcc-image id="mv-back" image="images/mv/mv-back.svg" style="width:200px">
   <trigger-dcc event="click" publish="clicked"></trigger-dcc>
</dcc-image>
~~~

~~~html
<img id="mv-front" src="images/mv/mv-front.svg" style="width:200px">
<img id="mv-back" src="images/mv/mv-back.svg" style="width:200px">
<dcc-web location="mv-back">
   <trigger-dcc event="click" publish="clicked"></trigger-dcc>
</dcc-web>
~~~


### Selective Publish/Subscribe

#### Topic Filters and Wildcards

In the subscription process, it is possible to specify a specific Topic Name or a Topic Filter, which works as a regular expression representing a set of possible Topic Names.

Wildcards are represented by the special `#` and/or `+` characters, appearing inside a Topic Name in the subscription process. They enable the subscription of a set of topics, since they generically represent one or more Topic Levels, according to the following rules:

#### Multilevel Wildcard `#`
The wildcard `#` can be used only in two positions in the Topic Filter:
* alone (the filter is only a `#`) - matches any Topic Name with any number of levels;
* end of the Topic Name (always preceded by a `/ `) -  matches any number of Topic Levels with the prefix specified before the wildcard.

#### Single Level Wildcard `+`
Only a single Topic Level can be matched by the wildcard  `+`, which represents any possible complete Topic Level Label. The `+` wildcard can appear only in four positions:
* alone (the filter is only a `+`) - matches any Topic Label in a single level (without slashes);
* beginning of the Topic Filter, always followed by a slash;
* end of the Topic Filter, always preceded by a slash;
* middle of the Topic Filter, always between two slashes.

The following example show messages selectively displayed.

~~~html
<dcc-trigger label="Disease"
             action="news/disease"
             value="new disease">
</dcc-trigger>

<dcc-trigger label="Medication"
             action="news/medication"
             value="new medication">
</dcc-trigger>

<dcc-lively-talk duration="0s"
                 character="doctor"
                 speech="I heard about a ">
  <subscribe-dcc topic="news/#"></subscribe-dcc>
</dcc-lively-talk>

<dcc-lively-talk duration="0s"
                 character="nurse"
                 speech="I heard about a ">
  <subscribe-dcc topic="news/disease"></subscribe-dcc>
</dcc-lively-talk>

<dcc-lively-talk duration="0s"
                 character="patient"
                 speech="I heard about a ">
  <subscribe-dcc topic="news/soccer"></subscribe-dcc>
</dcc-lively-talk>
~~~

## Notice or Input (`<dcc-notice-input>`)

~~~html
<dcc-notice-input text="Hello">
</dcc-notice-input>

<dcc-notice-input text="Do you agree?" buttona="Yes" buttonb="No">
</dcc-notice-input>

<dcc-notice-input itype="input" text="Type your name:">
</dcc-notice-input>
~~~

## Timer

~~~html
<dcc-trigger label="Start" action="timer/start">
</dcc-trigger>

<dcc-timer cycles="10" interval="1000" publish="send/cycle">
   <subscribe-dcc topic="timer/start" role="start"></subscribe-dcc>
</dcc-timer>

<dcc-lively-talk id="doctor"
                 duration="0s"
                 character="doctor"
                 speech="Counting: ">
  <subscribe-dcc topic="send/cycle"></subscribe-dcc>
</dcc-lively-talk>
~~~

## Select a State

~~~html
Doctor, I am feeling chest pain since yesterday. The <dcc-state-select id="dcc1" states=" ,k,+,=,-" answer="=">pain is continuous</dcc-state-select> and {is located just in the middle of my chest}/=/, {worsening when I breathe}/+/ and {when I lay down on my bed}/+/. I have {arterial hypertension}/-/ and {I smoke 20 cigarettes}(smoking=20/day)/-/ every day.

Doctor, I am feeling chest pain since yesterday. The <dcc-state-select id="dcc1" states=" ,k,+,=,-">pain is continuous</dcc-state-select> and <dcc-state-select id="dcc2" states=" ,k,+,=,-">is located just in the middle of my chest</dcc-state-select> <dcc-state-select id="dcc3" states=" ,k,+,=,-">worsening when I breathe</dcc-state-select> and <dcc-state-select id="dcc4" states=" ,k,+,=,-">when I lay down on my bed</dcc-state-select>. I have <dcc-state-select id="dcc5" states=" ,k,+,=,-">arterial hypertension</dcc-state-select> and <dcc-state-select id="dcc6" states=" ,k,+,=,-">I smoke 20 cigarettes</dcc-state-select> every day.

Doctor, I am feeling chest pain since yesterday. The <dcc-state-select id="dcc1" states=" ,k,+,=,-" answer="=">pain is continuous</dcc-state-select> and <dcc-state-select id="dcc2" states=" ,k,+,=,-" answer="=">is located just in the middle of my chest</dcc-state-select> <dcc-state-select id="dcc3" states=" ,k,+,=,-" answer="+">worsening when I breathe</dcc-state-select> and <dcc-state-select id="dcc4" states=" ,k,+,=,-" answer="+">when I lay down on my bed</dcc-state-select>. I have <dcc-state-select id="dcc5" states=" ,k,+,=,-" answer="-">arterial hypertension</dcc-state-select> and <dcc-state-select id="dcc6" states=" ,k,+,=,-" answer="-">I smoke 20 cigarettes</dcc-state-select> every day.
~~~

~~~html
<dcc-group-select states=" ,k,+,=,-">
Doctor, I am feeling chest pain since yesterday. The <dcc-state-select id="dcc1">pain is continuous</dcc-state-select> and <dcc-state-select id="dcc2">is located just in the middle of my chest</dcc-state-select> <dcc-state-select id="dcc3">worsening when I breathe</dcc-state-select> and <dcc-state-select id="dcc4">when I lay down on my bed</dcc-state-select>. I have <dcc-state-select id="dcc5">arterial hypertension</dcc-state-select> and <dcc-state-select id="dcc6">I smoke 20 cigarettes</dcc-state-select> every day.
</dcc-group-select>
~~~
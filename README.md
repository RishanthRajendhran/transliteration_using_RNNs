<h1>Recurrent Neural Network based seq2seq model for transliteration task</h1>
<h3>Team Members</h3>
  <ol>
    <li><strong>Rishanth. R</strong> (CS18B044)</li> 
  </ol>
  <h3>Important Note</h3>
  <p>
      Due to 100MB file size limit of Github, data files (dakshina_dataset_v1.0) could not be uploaded in this github repository.
</p>
<p>
      Due to the large sizes of certain data files, the following error was encountered when trying to push code from local repository to online repository:<br/>
      <em>"fatal: the remote end hung up unexpectedly"</em><br/>
      Because of this error, several new folders had to be created and the original version control couldn't be retained in the current repository. 
</p>
<h3>Contents of the files</h3>
  <ul>
  <li>
    <strong>seq2seq.py</strong>
    <p>
      Contains the class definition for a Recurrent Neural Network based seq2seq model with all the associated functions required to initialise an RNN as per specified configuration, train the RNN over the training set while using the validation set to find the hyperparameters and to evaluate the model over the test set. The function to visualise the attention weights in case of a model with attention is also present in this file. 
    </p>
    <p>
      Also contains the class definition for an AttentionLayer based on Bahdanau et al.
    </p>
  </li>
  <li>
    <strong>train.py</strong>
    <p>
        This file generates encoder and decoder inputs from the dakshina dataset and then builds an RNN based seq2seq model with specified configuration. The RNN is then trained using the training data. <br/>
        The "-addAttention" flag has to be used while running the program to enable attention in the RNN model. Note that when attention is set, only single layer of encoder and decoder is supported. <br/>
        The  "-loadModel" flag has to be used while running the program if one wants to load model weights saved ( weights stored in "./best_model_acc' are used when attention is not set and './best_attn_model_acc' when it is) during a previous training session before training over the train dataset. 
    </p>
  </li>
  <li>
    <strong>test.py</strong>
    <p>
        This file generaes encoder and decoder inputs from the dakshina dataset and then builds an RNN with specified configuration. The RNN then makes predictions over the test dataset. For each prediction, the program prints the input encoder sequence, the expected decoded sequence and the actual decoder output. It also displays whether the prediction was correct. Information regarding number of sequences tested, number of perfect matches and number of partial matches also get printed. At the end, the word-level accuracy (perfect matches only) is displayed. <br/>
        Due to several mistakes in output dataset, a leeway is given to the model. If the number of corrections to be made to convert the decoder output to the expected output sequence is lesser than 2 and if the length of the decoder output and the expected sequence is at least twice as large as the number of corrections to make, it is considered a partial match. 
    </p>
    <p>
      When running this program in the default test mode (i.e. w/o the "-visualiseAttention" flag), the predictions made on the test data set is stored in the file "predictions_vanilla.txt" ("predictions_attention.text" in case of an attention based model) in the same folder. 
    </p>
    <p>
      By default, the program when run evaulates a previously built, trained and saved model (saved in ./best_model_acc in case of models without attention and in ./best_attn_model_acc in case of models with attention). If the flag "-loadModel" is not set while running the program, the program assumes that the user has forgotten to train the model first and returns an error. <br/>
      Run this file only after running train.py. Do not forget to set "-loadModel" while running this file. 
    </p>
    <p>
        To test a model with attention, the "-addAttention" flag has to be set while running the program. 
    </p>
    <p>
      To plot the confusion matrix and (number of corrections required vs number of test instances), run this program with the flag "-plotConfusion".
    </p>
    <p>
      To only visualise the attention weights in case of a model with attention (Remember to set "-addAttention" flag while running this file), the "-visualiseAttention" flag has to be used while running this program. <br />
      Note that the default behaviour of this program is to only make predictions of a previously built model over the test dataset. If one wants to visualise the attention weights of a model with attenion, the "-visualiseAttention" flag  must be set. When this flag is set, the program will not make predictions over the test data set. 
    </p>
    <p>
      For attention models, both text attention visualisation, which shows the top 3 predictions made for every input character in a given input sequence and also the determining character in the input encoder sequence for a given character in the decoder sequence, and text confidence visuluation, which shows what parts of the decoded sequence fired every cell in the final decoder layer, can be obtained. <br/>
      To obtain text attention visualisation, run this file as follows:  "python3 test.py -loadModel -addAttention -visualiseTextAttention" <br/>
      The text attention visualisation output will be saved as a HTML file "textAttentionResult.html" in the current working directory, and can be viewed on a web browser like Safari/Chrome <br/>
      To obtain text confidence visualisation, run this file as follows:  "python3 test.py -loadModel -addAttention -visualiseTextConfidence" <br/>
      The text confidence visualisation output will be saved as a HTML file "textConfidenceResult.html" in the current working directory, and can be viewed on a web browser like Safari/Chrome <br/>
      <strong>Sample Text Confidence Visualisation for an Attention-based Model</strong>
      <img src="./sampleTextConfidenceVisualisation.png" alt="sampleTextConfidenceVisualisation"/>
      <strong>Interesting observations:</strong>
      <ul>
        <li>
          Cell 8 fires for all characters except the first few characters in the input sequence.
        </li>
        <li>
          Cell 36 fires for the last few characters of long input sequences.
        </li>
        <li>
          Cell 39 fires for the first few (4-5) characters of all input sequences.
        </li>
        <li>
          Cell 40 fires for the first 2-3 characters of all input sequences.
        </li>
        <li>
          Cell 41 fires for almost every phonetic character sequences in the input sequence. <br/>
          E.g. (r)a(s)a(ay)a(n)a(n)ga(l)i(n)dri for (இர)(ச)(ாய)(ண)(ங்)க(ள)ி(ன்)றி where character sequences with high activations have been enclosed in brackets. <br/>
          (இர) => (r) <br/>
          (ச)  => (s) <br/>
          (ாய) => (ay) <br/>
          (ண) => (n) <br/>
          (ங்) => (n) <br/>
          (ள) => (l) <br/>
          (ன்) => (n) <br/>
        </li>
        <li>
          Cell 46 did not fire for different sounds with 't/d' as base. <br/>
          E.g. Characters enclosed in bracket did not fire<br/>
          rasaayanangalind(r)i => (dri) sound <br/>
          kat(tu)ppad(u)thapaduvat(h)aal => (tu), (du) and (th) sounds <br/>
          rasaayana => fired for entire sequence due to absence of 't/d'-based sounds<br/>
          pinvilaivugalaana => fired for entire sequence due to absence of 't/d'-based sounds<br/>
          kaat(r)u => (tru) sound<br/>
          maasupad(u)vat(h)u,t(h)anneer => (du), (th) and (th) sounds<br/>
          maasupaad(u)velan => (du) sound<br/>
          nilangal => fired for entire sequence due to absence of 't/d'-based sounds<br/>
          maasupad(u)vat(tr)u,marum => (du) and (tru) sounds<br/>
          pira => fired for entire sequence due to absence of 't/d'-based sounds<br/>
        </li>
      </ul>
    </p>
    <p>
      For non-attention models, only text confidence visuluation, which shows what parts of the decoded sequence fired every cell in the final decoder layer, can be obtained. <br/>
      To obtain text confidence visualisation, run this file as follows:  "python3 test.py -loadModel -visualiseTextConfidence" or "python3 test.py -loadModel -visualiseTextAttention" <br/>
      The text confidence visualisation output will be saved as a HTML file "textConfidenceResult.html" in the current working directory, and can be viewed on a web browser like Safari/Chrome <br/>
    </p>
  </li>
  <li>
    <strong>textAttentionTemplate.html</strong>
    <p>
      Contains the HTML code for generating text attention visualisations. Used by test.py internally when generating text attention visualisations.
    </p>
  </li>
  <li>
    <strong>textConfidenceTemplate.html</strong>
    <p>
      Contains the HTML code for generating text confidence visualisations. Used by test.py internally when generating text confidence visualisations.
    </p>
  </li>
  <li>
    <strong>sweep.yaml</strong>
    <p>
      Contains the sweep configuration.<br/>
      The bayes strategy was chosen for the sweep over grid search (computationally expensive) and random search (might settle for local minima)<br/>
      Categorical accuracy of the validation set is used to tune the hyperparamters with the objective of maximising it.
    </p>
  </li>
  <li>
    The folder might contain 4 additional folders with the weights of previously trained models:
    <ul>
      <li><b>best_model_acc</b> : best model without attention based on validation categorical accuracy</li>
      <li><b>best_model_loss</b> : best model without attention based on validation categorical loss</li>
      <li><b>best_attn_model_acc</b> : best model with attention based on validation categorical accuracy</li>
      <li><b>best_attn_model_loss</b> : best model with attention based on validation categorical loss</li>
    </ul>
    Note that these models might not be the best possible models. The best version of the last trained model is stored in these folders. 
  </li>
 </ul>
 <p>
    <b>Note:</b><br/>
    If there are files in the folder but not mentioned above, ignore them.
 </p>
<h3>Running the code</h3>
  <h5>Train and evaluate a recurrent neural network based seq2seq model for transliteration task</h5>
  <p>
    To train a reccurent neural network based seq2seq model for a specific set of hyperparamters, one has to first modify the hyperparameters_default dictionary, defined in train.py, 
    appropriately.<br/>
    After logging into your wand accound using 'wandb login' and modifying run information in 'train.py', run the command 'python3 train.py' in the project directory in the terminal.<br/>
    The code initialises a recurrent neural network based seq2seq model with the specified hyperparameters, and then trains the RNN over the training dataset. The best version of the current model is stored in aforementioned folders.<br/>
    Note: Use appropriate flags while running this file <br/>
    To evaluate the program over the test dataset, run the command 'python3 test.py -loadModel'.<br/>
    <h5>Visualise the attention weights for a recurrent neural network  with attention based seq2seq model for transliteration task</h5>
    To visualise the attention weights of a saved attention model, run the command 'python3 test.py -loadModel -addAttention -visualiseAttention'.<br/>
    <h5>Visualise the text attention for a recurrent neural network  with attention based seq2seq model for transliteration task</h5>
    To visualise the text attentions of a saved attention model, run the command 'python3 test.py -loadModel -addAttention -visualiseTextAttention'<br/>
    <h5>Visualise the text confidence for a recurrent neural network  with attention based seq2seq model for transliteration task</h5>
    To visualise the text confidence of a saved attention model, run the command 'python3 test.py -loadModel -addAttention -visualiseTextConfidence'.<br/>
    <h5>Visualise the text confidence for a recurrent neural network  with non-attention based seq2seq model for transliteration task</h5>
    To visualise the text confidence of a saved non-attention model, run the command 'python3 test.py -loadModel -visualiseTextConfidence' or 'python3 test.py -loadModel -visualiseTextAttention'.<br/>
  </p>
  <h5>To plot confusion maps for the test data</h5>
  <p>
    Run the following command to generate confusion heatmaps and (number of corrections reequired) vs (number of test instances) plot: <br/>
    "python3 test.py -loadModel -plotConfusion" <br/>
    Note: Use the "-addAttention" flag for attention-based model.
  </p>
  <h5>Run a sweep</h5>
  <p>
    Run the command 'wandb sweep sweep.yaml' in the project directory in the terminal.<br/>
    Then run the command 'wandb agent %sweep-agent-generated-by-previous-command-here%' to start the sweep.
  </p>


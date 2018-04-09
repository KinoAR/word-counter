<template>
  <div class="row justify-content-center">
    <div class="col-8">
      <h2>Word Counter</h2>
        <div class="col-xs-8">
          <form>
          <div class="form-group">
            <label for="WordCounter">Enter Text</label>
            <textarea v-model="text" class="form-control" id="textArea" rows="9" cols="80">
            </textarea>
          </div>
        </form>
        </div> 
    </div>
    <div class="col-4 ui-padding">
      <div class="panel panel-primary blue-header">
        <div class="panel-heading">Details</div>
        <div class="panel-body">
          <ul class="list-group list-text">
            <li class="list-group-item">
              Words: <span class="badge">{{wordCount}}</span>
            </li>
            <li class="list-group-item">
              Characters: <span class="badge">{{characterCount}}</span>
            </li>
            <li class="list-group-item">
              Sentences: <span class="badge">{{sentenceCount}}</span>
            </li>
            <li class="list-group-item">
              Grade Level: <span class="badge">{{gradeLevel}}</span>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Main",
  data() {
    return {
      msg: 'Main application',
      wordCount: 0,
      characterCount: 0,
      sentenceCount: 0,
      syllables: 0,
      readingTime: 0, //Reading Time 200 - 250 words per minute
      gradeLevel: 'Unknown',
      text: ''
    }
  },
  watch: {
    text(val) {
      this.updateWord(val);
    }
  },
  methods: {
    updateWord(value) {
      if(value !== null) {
        try {
          const words = value.match(/\b(\w+)\b/g);
          const chars = value.split("");
          const sentences = value.match(/\(?[^\.\?\!]+[\.!\?]\)?/g);
          const syllables = value.match(/\b(\w+)\b/g);
          let syllableCount = 0;
          if(syllables !== null)
          syllableCount = syllables.map(this.syllableCount)
            .reduce((previous, current) => {
              return previous + current;
            }, 0);
          
          this.characterCount = chars !== null ? chars.length : this.characterCount;
          this.wordCount = words !== null ?  words.length : this.wordCount;
          this.sentenceCount = sentences !== null ? sentences.length : this.sentenceCount;
          this.syllables = syllables !== null ? syllableCount : this.syllables;
          this.gradeLevel = this.readingLevel(this.syllables, this.wordCount, this.sentenceCount);
        }
        catch(err) {
          console.error(err);
        }
      }
    },
    readingLevel(syllables, wordCount, sentenceCount) {
      const gradeScore = 206.835 - 1.015 * (wordCount/sentenceCount) - 84.6 * (syllables/wordCount);
      return (gradeScore > 90 && gradeScore < 101) ? '5th Grade'
      : (gradeScore > 80 && gradeScore < 90) ? '6th Grade' 
      : (gradeScore > 70 && gradeScore < 80) ? '7th Grade'
      : (gradeScore > 60 && gradeScore < 70) ? '8th & 9th Grade'
      : (gradeScore > 50 && gradeScore < 60) ? '10th & 12th Grade'
      : (gradeScore > 30 && gradeScore < 50) ? 'College'
      : (gradeScore > 0 && gradeScore < 30) ? 'College Graduate'
      : 'Unknown';
    }, 
    syllableCount(word) {
      word = word.toLowerCase();                               
      if(word.length <= 2) {
         return 1; 
      }                             
        word = word.replace(/(?:[^laeiouy]es|ed|[^laeiouy]e)$/, '');  
        word = word.replace(/^y/, '');
        const matchArray = word.match(/[aeiouy]{1,2}/g);
        return matchArray !== null ? matchArray.length : 0;
    }
  }
}
</script>

<style scoped>
.ui-padding {
  margin-top: 80px;
}
.blue-header {
  background-color: #337ab7;
  border-top-left-radius: .25rem;
  border-top-right-radius: .25rem;
  border-bottom-right-radius: .25rem;
  border-bottom-left-radius: .25rem;
  color:white;
}
.list-text {
  color: black;
}
#textArea {
  resize: none;
}
</style>

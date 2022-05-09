<template>
  <div style="padding: 2rem; max-width: 1366px; margin: 0 auto">
    <div
      style="
        position: fixed;
        display: flex;
        justify-content: center;
        width: 100%;
      "
    >
      <div style="display: flex; justify-content: center; padding: 30px">
        <input
          type="text"
          v-model.trim="searchTerm"
          style="height: 40px; width: 320px; font-size: 18px"
        />
      </div>
    </div>

    <table class="table" style="margin-top: 96px">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Question</th>
          <th scope="col">Correct Answer</th>
          <th scope="col">Wrong Answer</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="question in quizFilteredSuggestion" :key="question.id">
          <td class="center">{{ question.id }}</td>
          <td>
            <div style="padding: 8px">
              <div
                class="border correct"
                v-html="
                  highlightTerm(
                    highlightSearch(question.correctQuestion, searchTerm),
                    question.correctAnswer
                  )
                "
              ></div>
            </div>
            <div style="padding: 8px">
              <div
                class="border wrong"
                v-html="
                  highlightSearch(
                    highlightTerm(question.wrongQuestion, question.wrongAnswer),
                    searchTerm
                  )
                "
              ></div>
            </div>
          </td>
          <td class="center">
            {{ question.correctAnswer }}
          </td>

          <td class="center">
            {{ question.wrongAnswer }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import quizData from "../assets/quiz-answers.json";
export default {
  data() {
    return {
      quizAnswer: quizData,
      searchTerm: "",
    };
  },

  computed: {
    quizFilterMulti() {
      const query = this.searchTerm.toLowerCase();
      const multiQuery = query.split(";");
      if (multiQuery.length > 0) {
        return this.quizAnswer.filter((q) => {
          return multiQuery.some((v) => {
            return (
              q.correctQuestion.toLowerCase().includes(v) &&
              q.wrongQuestion.toLowerCase().includes(v) &&
              (
                q.correctQuestion.toLowerCase() +
                " " +
                q.wrongQuestion.toLowerCase()
              ).includes(v)
            );
          });
        });
      } else {
        return this.quizFilteredSuggestion;
      }
    },

    quizFilteredSuggestion() {
      const query = this.searchTerm.toLowerCase();
      const multiQuery = query.split(";");

      return this.quizAnswer.filter((q) => {
        return (
          q.correctQuestion.toLowerCase().includes(query) ||
          q.wrongQuestion.toLowerCase().includes(query)
        );
      });
    },
  },

  setup() {
    const highlightTerm = (str, term) => {
      if (term.trim() === "") {
        return str;
      }

      return str.replace(new RegExp(term, "gi"), `<b>${term}</b>`);
    };

    const highlightSearch = (str, term) => {
      if (term.trim() === "") {
        return str;
      }

      let regex = new RegExp(term, "gi");

      return str.replace(regex, `<span class="highlightColor">${term}</span>`);
    };

    return {
      highlightTerm,
      highlightSearch,
    };
  },
};
</script>


<style scoped>
.table {
  width: 100%;

  border: 2px solid #2b2b2b;
  color: #ddd;
}
.table th {
  padding-top: 12px;
  padding-bottom: 12px;
  background-color: #000;
}
.table tbody td {
  background: #161616;
}

.center {
  text-align: center;
}

.border {
  padding: 6px;
  border-radius: 2px;
  border-width: 1px;
  border-style: solid;
  padding-top: 12px;
  padding-bottom: 12px;
  opacity: 0.8;
}

.correct {
  border-color: rgb(96 165 250);
  background-color: rgb(12 74 110);
}

.wrong {
  border-color: rgb(251 113 133);
  background-color: rgb(136 19 55);
}
</style>
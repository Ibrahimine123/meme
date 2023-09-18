<script lang="ts">
  import { getRandomMeme, memes } from "./memes";

  function putPlace(where: string, element: Element) {
    const place = document.getElementById(where);
    place?.insertAdjacentElement("afterend", element);
  }

  function riddleReveal(answer: string) {
    const revealAnswer = document.createElement("button");
    revealAnswer.textContent = "Reveal Riddle Answer";
    revealAnswer.id = "reveal-answer";
    putPlace("riddleBtn", revealAnswer);

    revealAnswer.addEventListener("click", () => {
      const riddleAnswer = document.createElement("p");
      riddleAnswer.textContent = answer;
      putPlace("un-riddle", riddleAnswer);
      revealAnswer.remove();
    });
  }

  function displayMeme<T extends keyof HTMLElementTagNameMap>(
    element: T,
    where: string,
    memeType?: string
  ) {
    const container = document.querySelector(".content") as HTMLElement;
    const pTags = container.querySelectorAll("p");
    pTags.forEach((x) => x.remove());
    const riddleBtn = document.getElementById(
      "reveal-answer"
    ) as HTMLButtonElement;
    if (riddleBtn) riddleBtn.remove();

    const imgs = container?.querySelector("img");
    if (imgs) imgs.remove();

    const elem = document.createElement(element);

    if (element === "img" && elem instanceof HTMLImageElement) {
      const memeImgLink = getRandomMeme(memes.memesPic);
      elem.src = memeImgLink;
      putPlace(where, elem);
    }

    switch (memeType) {
      case "joke":
        const joke = getRandomMeme(memes.jokes);
        elem.textContent = joke;
        putPlace(where, elem);
        break;

      case "quote":
        const { quote, author } = getRandomMeme(memes.quotes);
        const authorPara = document.createElement("p");
        elem.textContent = quote;
        elem.id = "quotePara";
        putPlace("quotes", elem);
        authorPara.textContent = `- ${author}`;
        putPlace("quotePara", authorPara);
        break;

      case "riddle":
        const { question, answer } = getRandomMeme(memes.riddles);
        elem.textContent = question;
        elem.id = "un-riddle";
        putPlace("riddles", elem);
        riddleReveal(answer);
        break;
    }
  }
</script>

<h1>Actions</h1>

<div>
  <button on:click={() => displayMeme("img", "memes-pic")}>Show a meme</button>
  <button on:click={() => displayMeme("p", "jokes", "joke")}
    >Tell me a joke</button
  >
  <button on:click={() => displayMeme("p", "quotes", "quote")}
    >Random Quote</button
  >
  <button id="riddleBtn" on:click={() => displayMeme("p", "riddles", "riddle")}
    >Riddle me</button
  >
</div>

<style lang="scss">
  div {
    display: flex;
    flex-direction: column;
  }
  h1 {
    text-align: center;
  }
</style>

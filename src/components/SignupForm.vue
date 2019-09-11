<template>
  <div class="content-box">
    <div class="newsletter-wrapper">
      <h2 class="newsletter-header">Join the Newsletter</h2>
      <template v-if="!subscribed">
        <p>Subscribe to get my latest content.</p>
        <form
          class="newsletter-form"
          action="https://dulisz.us19.list-manage.com/subscribe/post?u=10ee51b4b3eb672c84c21f2fe&amp;id=56ef7caf8a"
          method="post"
          target="_blank"
          @submit="markAsSubsribed"
        >
          <!-- <label for="firstName">First Name</label> -->
          <input
            name="NAME"
            type="text"
            placeholder="First Name"
            id="firstName"
          />
          <!-- <label for="email">Email</label> -->
          <input
            required
            name="EMAIL"
            type="text"
            placeholder="Email"
            id="email"
          />
          <div style="position: absolute; left: -5000px;" aria-hidden="true"><input type="text" name="b_10ee51b4b3eb672c84c21f2fe_56ef7caf8a" tabindex="-1" value=""></div>
          <input type="submit" value="Subscribe"/>
        </form>
        <small class="small">No spam. You can unsubscribe at any time.</small>
      </template>
      <p v-else transition="fade">
        Thank you for subscribing!
      </p>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      subscribed: false
    }
  },
  methods: {
    markAsSubsribed () {
      setTimeout(() => {
        this.subscribed = true
        localStorage.setItem('subscribed', true)
      }, 200)
    }
  },
  mounted () {
    this.subscribed = localStorage.getItem('subscribed') || false
  }
}
</script>

<style lang="scss" scoped>
.newsletter-wrapper {
  padding: 1rem;
  background: #f9f9f9;
  color: #444a51;
  box-shadow: var(--box-shadow);
  border-radius: var(--radius);
  transition: all .3s ease;

  &:hover {
    box-shadow: var(--box-shadow-hover)
  }

  p {
    margin-bottom: 0.5rem;
  }

  h2 {
    color: #444a51;
    font-size: 1.3rem;
    margin: 0;
    margin-bottom: 0.5rem;
  }

  small {
    font-size: 0.8rem;
    opacity: 0.7;
  }
}

.newsletter-form {
  display: flex;
  width: 100%;

  @media screen and (max-width: 650px) {
    display: block;
  }

  label {
    display: block;
    font-size: 0.7rem;
    line-height: 1;
    font-weight: 700;
    margin-top: 1rem;
  }

  input {
    margin-top: 0.4rem;
    padding: 0.4rem 0.7rem;
    font-size: 0.9rem;
    border: 1px solid #e5e5e5;
    margin-right: 0.5rem;
    margin-bottom: 0.4rem;
    border-radius: var(--radius);

    &:focus {
      border-color: var(--link-color);
    }
  }

  [type="submit"] {
    border: 1px solid var(--link-color);
    background: var(--link-color);
    color: #fff;
    padding: 0.4rem 1rem;
    font-weight: 700;
    border-radius: var(--radius);
    transition: all 0.1s ease;
    cursor: pointer;

    &:hover {
      border-color: #2b9759;
      background: #2b9759;
    }
  }
}
</style>

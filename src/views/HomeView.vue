<template>
	<div>
		<GreetingHeader :profileIamge="content.profileImage"
			:headerBackgroundImage="imageSrc(content.headerBackgroundImage)" />
		<NavBar :includePress="includePress" @navClicked="refName => scrollMeTo(refName)" />
		<AnnouncementBanner />

		<div ref="about">
			<AboutMe :aboutMeText="content.aboutMeText" :aboutMeImage="content.aboutMeImage"
				:twitterLink="content.twitterLink" @goToContact="scrollMeTo('contact')" />
		</div>

		<div ref="shows">
			<div v-for="(show, index) in content.currentWorkList" :key="`${show.currentWorkTitle}-feature`" :class="index % 2 == 0
				? 'row flex-lg-row-reverse'
				: 'row flex-lg-row'
				">
				<div class="col-lg p-0">
					<b-carousel id="carousel-1"
						class="currentWorkCarousel d-flex justify-content-center align-items-center" v-model="slide"
						:interval="4000" controls fade background="#00303b" style="text-shadow: 1px 1px 2px #333">
						<b-carousel-slide v-for="(image, index) in show.currentWorkImages"
							:key="`currentWorkImages-${index}`"
							class="d-flex justify-content-center align-items-center">
							<template #img>
								<img class="d-block img-fluid" width="100%" :src="imageSrc(image.workImage)"
									:alt="image.workImageAlt" />
							</template>
						</b-carousel-slide>
					</b-carousel>
				</div>
				<b-container class="col-lg-6 px-5 d-flex flex-column justify-content-around">
					<h3 class="display-5 pt-4">{{ show.currentWorkTitle }}</h3>
					<p>{{ show.currentWorkText }}</p>
					<div>
						<h4 class="display-6" v-if="show.currentWorkReviews">
							Reviews
						</h4>
						<div class="d-flex flex-wrap justify-content-center">
							<div v-for="review in show.currentWorkReviews"
								:key="`currentWorkReview-${review.reviewPublisher}`" class="mx-3">
								<p class="p-0 m-0">
									{{ review.reviewContent }}
								</p>
								<p>{{ review.reviewPublisher }}</p>
							</div>
						</div>
					</div>
					<b-button variant="custom-primary" size="lg" class="m-4 mb-5 align-self-center book-btn"
						:href="show.currentWorkTickets" target="_blank">
						See More
					</b-button>
				</b-container>
			</div>
		</div>

		<div class="py-4 featured-shows-wrapper bg-opacity-50">
			<b-container>
				<h2 class="display-2">Featured Shows</h2>
				<div class="d-flex flex-wrap justify-content-center gap-2">
					<div class="col-lg-5 my-3" v-for="show in content.featuredShowsList"
						:key="`${show.showTitle}-card`">
						<b-card :img-src="imageSrc(show.showImageSrc)" :img-alt="show.showImageAlt" img-top
							class="featured-show-card featured-show-card-height">
							<b-card-body class="d-flex flex-column justify-content-between featured-show-card-height">
								<div>
									<b-card-title>{{
										show.showTitle
									}}</b-card-title>
									<b-card-sub-title class="mb-3">{{
										show.showCompany
									}}</b-card-sub-title>
								</div>

								<b-card-text>{{ show.showText }}</b-card-text>
								<b-card-text class="small">Image Copyright:
									{{ show.showImageCopyright }}</b-card-text>
							</b-card-body>
						</b-card>
					</div>
				</div>

				<h2 class="display-2">Credits</h2>
				<p>{{ content.directorCredits }}</p>
				<p>{{ content.otherCredits }}</p>
				<b-button size="lg" variant="custom-secondary" class="my-4" :href="imageSrc(content.cvFile)"
					target="_blank">
					See Full CV
					<b-icon-file-earmark-medical-fill class="ms-2"></b-icon-file-earmark-medical-fill>
				</b-button>
			</b-container>
		</div>

		<div ref="press" v-if="content.pressList && content.pressList.length > 0">
			<b-container class="py-4 px-4 px-md-0">
				<h2 class="display-2">Press & Articles</h2>
				<div v-for="(article, index) in content.pressList" :key="`press-card-${index}`"
					class="row justify-content-center py-3 px-2 my-3 press-card">
					<div class="col-sm align-self-center">
						<b-card-title class="press-text">
							{{ article.pressQuote }}
							<span class="text-muted small">{{ article.pressPublisher }}</span>
						</b-card-title>
					</div>
					<b-button class="col-sm-2 mt-2 mt-sm-0 align-self-center" variant="custom-primary"
						:href="article.pressLink" target="_blank">Read</b-button>
				</div>
			</b-container>
		</div>

		<div class="pt-4 pb-3 footer-wrapper">
			<b-container ref="contact">
				<h2 class="display-2 text-white mb-4">Get In Touch</h2>
				<b-container v-if="content.agentsList && content.agentsList.length > 0" class="p-0 mb-4 contact-form">
					<h3 class="display-6 text-white text-start mb-3">Represented by</h3>
					<div class="d-flex flex-wrap justify-content-start gap-0 mb-3">
						<div v-for="agent in content.agentsList" :key="`agent-${agent.agentName}`"
							class="text-white text-start me-4 mb-3">
							<p class="my-0 fw-bold">{{ agent.agentName }}</p>
							<p class="my-0">Email: <a class="text-white" :href="`mailto:${agent.agentEmail}`">{{
								agent.agentEmail }}</a></p>
							<p class="my-0" v-if="agent.agentPhone">Phone: <a class="text-white"
									:href="`tel:${agent.agentPhone}`">{{ agent.agentPhone }}</a></p>
						</div>
					</div>
				</b-container>
				<b-form class="contact-form" name="contact" method="post" data-netlify="true"
					data-netlify-honeypot="bot-field">
					<h3 v-if="content.agentsList && content.agentsList.length > 0" class="display-6 text-white text-start">Or Message Me Directly</h3>
					<input type="hidden" name="form-name" value="contact" />
					<b-input class="my-3" placeholder="Name" name="name" autocomplete="name"
						v-model="contactForm.name"></b-input>
					<b-input class="my-3" placeholder="Email" name="email" type="email" autocomplete="email"
						v-model="contactForm.email"></b-input>
					<b-textarea class="my-3" placeholder="Message" name="message"
						v-model="contactForm.message"></b-textarea>
					<b-button variant="custom-primary" type="submit">Submit</b-button>
				</b-form>

				<div class="site-info mt-4">
					<p class="my-0">Copyright {{ new Date().getFullYear() }}</p>
					<a class="my-0 site-info" href="https://olliechurch.co.uk" target="_blank">
						Website built and maintained by Ollie C
					</a>
				</div>
			</b-container>
		</div>
	</div>
</template>

<script>
import contentJson from '../contentManagement/content.json'
import NavBar from '@/components/NavBar.vue'
import GreetingHeader from '@/components/GreetingHeader.vue'
import AboutMe from '@/components/AboutMe.vue'
import AnnouncementBanner from '@/components/AnnouncementBanner.vue'

export default {
	name: 'HomeView',

	components: {
		GreetingHeader,
		NavBar,
		AnnouncementBanner,
		AboutMe
	},

	data: function () {
		return {
			content: contentJson,
			slide: 0,
			contactForm: {
				name: '',
				email: '',
				message: ''
			}
		}
	},

	computed: {
		includePress() {
			return (
				this.content.pressList && this.content.pressList.length > 0
			)
		}
	},

	methods: {
		scrollMeTo(refName) {
			const element = this.$refs[refName]
			const top = element.offsetTop - 100

			window.scrollTo(0, top)
		},

		imageSrc(image) {
			return image.split('/public').join('')
		},

		getWorkClasses(index) {
			if (index % 2 == 0) {
				return 'row flex-lg-row-reverse'
			} else {
				return 'row flex-lg-row'
			}
		}
	}
}
</script>

<style scoped>
.book-btn {
	width: fit-content;
}

.currentWorkCarousel {
	width: 100%;
	height: 100%;
	object-fit: fill;
	overflow: hidden;
}

.featured-shows-wrapper {
	background: #fecf95;
}

.featured-show-card {
	border: 1px solid #00303b;
	box-shadow: 0px 1px 5px #00303b;
}

.featured-show-card-height {
	height: 100%;
}

.press-card {
	border-radius: 10px;
	border: 2px solid #00303b;
	box-shadow: 0px 1px 5px #00303b;
}

.press-text {
	text-align: left;
}

.footer-wrapper {
	background: #00303b;
}

.contact-form {
	max-width: 650px;
	margin: 0 auto;
}

.site-info {
	color: white !important;
	opacity: 0.75;
}
</style>

<template>
  <div class="grey lighten-5" style="font-family: 'Noto Sans KR', sans-serif">
    <v-container class="white">
      <v-row justify="center">
        <v-col cols="auto" style="padding-bottom: 90px">
          <v-card width="460">
            <v-card-text class="text-center px-12 py-16">
              <div class="text-h4 font-weight-black mb-10">
                관리자 코드 입력
              </div>
              <div class="d-flex">
                <v-img
                  :src="githubIconSrc"
                  width="120"
                  class="mx-auto mb-6"
                ></v-img>
              </div>

              <v-text-field
                v-model="adminCode"
                label="관리자 코드"
                type="password"
                variant="outlined"
                class="mb-4"
              ></v-text-field>

              <v-btn
                block
                x-large
                rounded
                color="primary"
                class="mt-4"
                @click="verifyAdminCode"
              >
                확인
              </v-btn>

              <v-btn
                block
                x-large
                rounded
                color="gray lighten-1"
                class="mt-4"
                @click="cancel"
              >
                취소
              </v-btn>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
import githubIconSrc from "@/assets/images/fixed/icon-github.svg";
import { useGithubAuthenticationStore } from "../../../github/stores/githubAuthenticationStore";
import { useHead } from '@vueuse/head'

// ✅ SEO 메타 정보 설정 1
useHead({
  title: "관리자 코드 입력 | 잡스틱(JobStick)",
  meta: [
    { name: "description", content: "잡스틱(JobStick)에서 GitHub 로그인을 진행하기 위해 관리자 코드를 입력하세요." },
    { name: "keywords", content: "관리자, 관리자 코드, JobStick, 잡스틱, job-stick, 관리자 로그인, 개발자 플랫폼, 개발자 취업, AI 면접" },
    { name: "robots", content: "noindex, nofollow" },
    { property: "og:title", content: "관리자 로그인 인증 - 잡스틱(JobStick)" },
    { property: "og:description", content: "잡스틱(JobStick) 관리자 인증을 통해 GitHub 로그인을 시작하세요." },
    { property: "og:image", content: "/images/og-admin-login.png" }
  ]
})


const router = useRouter();
const githubAuthentication = useGithubAuthenticationStore();
const adminCode = ref("");

// 관리자 코드 검증 요청
const verifyAdminCode = async () => {
  const isValid = await githubAuthentication.requestAdminCodeToDjango(
    adminCode.value
  );
  console.log("isValid:", isValid);

  if (isValid) {
    console.log("관리자 코드 인증 성공");
    await githubAuthentication.requestGithubLoginToDjango();
  } else {
    console.error("관리자 코드 인증 실패");
    alert("잘못된 관리자 코드입니다.");
  }
};

// 취소 버튼 (이전 페이지로 이동)
const cancel = () => {
  router.push("/account/login");
};
</script>

<style scoped>
/* 추가적인 스타일 필요 시 여기에 작성 */
</style>

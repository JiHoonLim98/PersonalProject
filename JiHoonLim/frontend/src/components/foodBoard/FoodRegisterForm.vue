<template>
  <div style="font-family: 'Noto Sans KR', sans-serif">
    <v-container class="white">
      <v-row justify="center">
        <v-col cols="auto">
          <v-card width="1300" style="margin: 30px">
            <v-card-text>
              <validation-observer v-slot="{ invalid }">
                <form @submit.prevent="onSubmit">
                  <table>
                    <div class="text-h4 font-weight-black mb-10">
                      레시피 등록
                    </div>
                    <div>
                      <div
                        style="width: 900px; margin-bottom: 15px; display: flex"
                      >
                        <h2 class="mb-5 mr-5">대표 이미지 등록</h2>
                        <div>
                          <input
                            placeholder="대표"
                            type="file"
                            id="files"
                            ref="files"
                            multiple
                            v-on:change="handlerFileUpload()"
                          />
                        </div>
                      </div>

                      <div>
                        <div>
                          <v-img :src="image" class="preview" />
                        </div>
                        <div v-if="image == ''" class="previewText">
                          이미지 미리보기 <br />(1465 X 695 권장)
                        </div>
                      </div>
                      <div>
                        <tr>
                          <td>
                            <validation-provider
                              v-slot="{ errors }"
                              name="요리 이름"
                              :rules="{ required: true }"
                            >
                              <v-text-field
                                style="width: 500px"
                                v-model="name"
                                label="요리 이름"
                                clearable
                                outlined
                                color="orange"
                                :error-messages="errors"
                              />
                            </validation-provider>
                          </td>
                        </tr>

                        <tr>
                          <td>
                            <v-text-field
                              style="width: 300px"
                              v-model="userInfo.nickName"
                              label="작성자"
                              clearable
                              disabled
                              outlined
                              color="orange"
                            />
                          </td>
                        </tr>
                      </div>
                    </div>

                    <tr>
                      <td>
                        <validation-provider
                          v-slot="{ errors }"
                          name="한줄 설명"
                          :rules="{ required: true }"
                        >
                          <v-textarea
                            style="width: 700px"
                            v-model="des"
                            label="한줄 설명"
                            clearable
                            auto-grow
                            outlined
                            color="orange"
                            :error-messages="errors"
                          />
                        </validation-provider>
                      </td>
                    </tr>
                    <tr>
                      <td>
                        <validation-provider
                          v-slot="{ errors }"
                          name="재료"
                          :rules="{ required: true }"
                        >
                          <v-textarea
                            v-model="material"
                            clearable
                            label="재료"
                            auto-grow
                            outlined
                            color="orange"
                            :error-messages="errors"
                          ></v-textarea>
                        </validation-provider>
                      </td>
                    </tr>

                    <tr>
                      <td>
                        <validation-provider
                          v-slot="{ errors }"
                          name="요리 순서"
                          :rules="{ required: true }"
                        >
                          <v-textarea
                            v-model="content"
                            label="요리 순서"
                            clearable
                            auto-grow
                            outlined
                            color="orange"
                            :error-messages="errors"
                          ></v-textarea>
                        </validation-provider>
                      </td>
                    </tr>

                    <tr>
                      <td>
                        <v-textarea
                          v-model="tip"
                          label="팁 / 주의사항"
                          clearable
                          auto-grow
                          outlined
                          color="orange"
                        ></v-textarea>
                      </td>
                    </tr>

                    <tr>
                      <td class="d-flex">
                        <validation-provider
                          v-slot="{ errors }"
                          name="종류"
                          :rules="{ required: true }"
                        >
                          <v-select
                            v-model="chooseKind"
                            :items="kind"
                            item-text="value"
                            item-value="value"
                            label="종류"
                            dense
                            outlined
                            style="width: 110px"
                            class="mr-3"
                            color="orange"
                            :error-messages="errors"
                          ></v-select>
                        </validation-provider>
                        <validation-provider
                          v-slot="{ errors }"
                          name="재료"
                          :rules="{ required: true }"
                        >
                          <v-select
                            v-model="chooseMat"
                            :items="mat"
                            item-text="value"
                            item-value="value"
                            label="재료"
                            dense
                            outlined
                            style="width: 110px"
                            class="mr-3"
                            color="orange"
                            :error-messages="errors"
                          ></v-select>
                        </validation-provider>
                        <validation-provider
                          v-slot="{ errors }"
                          name="방법"
                          :rules="{ required: true }"
                        >
                          <v-select
                            v-model="chooseWay"
                            :items="way"
                            item-text="value"
                            item-value="value"
                            label="방법"
                            dense
                            outlined
                            style="width: 110px"
                            class="mr-3"
                            color="orange"
                            :error-messages="errors"
                          ></v-select>
                        </validation-provider>
                      </td>
                    </tr>
                  </table>

                  <div class="d-flex" style="justify-content: flex-end">
                    <v-btn
                      type="submit"
                      class="mr-5"
                      large
                      rounded
                      color="orange lighten-1"
                      :disabled="invalid"
                    >
                      등록
                    </v-btn>
                    <v-btn to="foodList" large rounded color="orange lighten-1">
                      취소
                    </v-btn>
                  </div>
                </form>
              </validation-observer>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  name: "FoodRegisterForm",

  data() {
    return {
      name: "",
      des: "",
      material: "",
      content: "",
      tip: "",
      files: "",
      response: "",
      image: "",
      userInfo: JSON.parse(localStorage.getItem("userInfo")),

      chooseKind: "",
      chooseMat: "",
      chooseWay: "",

      kind: [
        { value: "밥" },
        { value: "반찬" },
        { value: "국물" },
        { value: "면" },
        { value: "디저트" },
        { value: "분식" },
        { value: "샐러드" },
        { value: "음료" },
        { value: "기타" },
      ],
      mat: [
        { value: "소고기" },
        { value: "돼지고기" },
        { value: "닭고기" },
        { value: "채소" },
        { value: "해물" },
        { value: "계란" },
        { value: "유제품" },
        { value: "기타" },
      ],
      way: [
        { value: "구이" },
        { value: "찜" },
        { value: "국탕찌개" },
        { value: "볶음" },
        { value: "조림" },
        { value: "튀김" },
        { value: "무침 / 비빔" },
        { value: "기타" },
      ],
    };
  },
  created() {
    if (this.$store.state.userInfo == null) {
      alert("로그인 후 이용해주세요.");
      this.$router.push("/login");
    }
  },
  methods: {
    onSubmit() {
      this.writer = this.userInfo.nickName;
      const {
        name,
        des,
        material,
        content,
        writer,
        tip,
        chooseKind,
        chooseMat,
        chooseWay,
      } = this;
      const file = this.$refs.files.files[0];
      if (file == null) {
        alert("대표 이미지를 등록해주세요.");
        return;
      }
      this.$emit("submit", {
        name,
        des,
        material,
        content,
        writer,
        tip,
        chooseKind,
        chooseMat,
        chooseWay,
        file,
      });
    },
    handlerFileUpload() {
      var image = this.$refs["files"].files[0];
      const url = URL.createObjectURL(image);
      this.image = url;

      this.files = this.$refs.files.files[0];
    },
  },
};
</script>
<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400&display=swap");

.preview {
  max-width: 500px;
  max-height: 250px;
  width: 500px;
  height: 250px;
  position: absolute;
  right: 55px;
  top: 45px;
  border: 1px dotted black;
  background-color: lightgray;
}
.previewText {
  position: absolute;
  right: 250px;
  top: 150px;
  text-align: center;
}
</style>

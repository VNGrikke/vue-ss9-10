<template>
  <div class="w-[80%] m-auto mt-4 h-[100vh]">
    <main class="main">
      <header class="d-flex justify-content-between mb-3">
        <h3>Nhân viên</h3>
        <button class="btn btn-primary" @click="toggleEmployeeForm">
          Thêm mới nhân viên
        </button>
      </header>

      <div class="d-flex align-items-center justify-content-end gap-2 mb-3">
        <input
          v-model="searchQuery"
          style="width: 350px"
          type="text"
          class="form-control"
          placeholder="Tìm kiếm theo email"
        />
        <i
          class="fa-solid fa-arrows-rotate"
          title="Refresh"
          @click="resetSearch"
        ></i>
      </div>

      <!-- Danh sách nhân viên -->
      <table class="table table-bordered table-hover table-striped">
        <thead>
          <tr>
            <th>STT</th>
            <th>Họ và tên</th>
            <th>Ngày sinh</th>
            <th>Email</th>
            <th>Địa chỉ</th>
            <th>Trạng thái</th>
            <th colspan="2">Chức năng</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(employee, index) in filteredEmployees" :key="employee.id">
            <td>{{ index + 1 }}</td>
            <td>{{ employee.name }}</td>
            <td>{{ employee.dateOfBirth }}</td>
            <td>{{ employee.email }}</td>
            <td>{{ employee.address }}</td>
            <td>
              <div style="display: flex; align-items: center; gap: 8px">
                <div
                  :class="[
                    'status',
                    employee.status === 'active'
                      ? 'status-active'
                      : 'status-stop',
                  ]"
                ></div>
                <span>{{
                  employee.status === "active"
                    ? "Đang hoạt động"
                    : "Ngừng hoạt động"
                }}</span>
              </div>
            </td>
            <td>
              <span
                class="button button-block"
                @click="toggleBlockEmployee(employee)"
              >
                {{ employee.status === "active" ? "Chặn" : "Bỏ chặn" }}
              </span>
            </td>
            <td>
              <span class="button button-edit" @click="editEmployee(employee)"
                >Sửa</span
              >
            </td>
            <td>
              <span
                class="button button-delete"
                @click="deleteEmployee(employee)"
                >Xóa</span
              >
            </td>
          </tr>
        </tbody>
      </table>

      <footer class="d-flex justify-content-end align-items-center gap-3">
        <select class="form-select" v-model="recordsPerPage">
          <option value="10">Hiển thị 10 bản ghi trên trang</option>
          <option value="20">Hiển thị 20 bản ghi trên trang</option>
          <option value="50">Hiển thị 50 bản ghi trên trang</option>
          <option value="100">Hiển thị 100 bản ghi trên trang</option>
        </select>
        <ul class="pagination">
          <li class="page-item" @click="previousPage">
            <a class="page-link" href="#">Previous</a>
          </li>
          <li class="page-item" v-for="n in totalPages" :key="n">
            <a class="page-link" href="#" @click="setPage(n)">{{ n }}</a>
          </li>
          <li class="page-item" @click="nextPage">
            <a class="page-link" href="#">Next</a>
          </li>
        </ul>
      </footer>
    </main>

    <!-- Form thêm mới nhân viên -->
    <div v-if="showEmployeeForm" class="overlay">
      <form class="form" @submit.prevent="saveEmployee">
        <div class="d-flex justify-content-between align-items-center">
          <h4>
            {{ isEditing ? "Chỉnh sửa nhân viên" : "Thêm mới nhân viên" }}
          </h4>
          <i class="fa-solid fa-xmark" @click="toggleEmployeeForm"></i>
        </div>

        <div>
          <label class="form-label" for="userName">Họ và tên</label>
          <input
            id="userName"
            type="text"
            class="form-control"
            v-model="employeeForm.name"
            required
          />
        </div>

        <div>
          <label class="form-label" for="dateOfBirth">Ngày sinh</label>
          <input
            id="dateOfBirth"
            type="date"
            class="form-control"
            v-model="employeeForm.dateOfBirth"
            required
          />
        </div>

        <div>
          <label class="form-label" for="email">Email</label>
          <input
            id="email"
            type="email"
            class="form-control"
            v-model="employeeForm.email"
            required
          />
        </div>

        <div>
          <label class="form-label" for="address">Địa chỉ</label>
          <textarea
            class="form-control"
            id="address"
            rows="3"
            v-model="employeeForm.address"
            required
          ></textarea>
        </div>

        <div>
          <button type="submit" class="w-100 btn btn-primary">
            {{ isEditing ? "Cập nhật" : "Thêm mới" }}
          </button>
        </div>
      </form>
    </div>

    <!-- Modal xác nhận chặn tài khoản -->
    <div v-if="showBlockModal" class="overlay">
      <div class="modal-custom">
        <div class="modal-title">
          <h4>Cảnh báo</h4>
          <i class="fa-solid fa-xmark" @click="toggleBlockModal"></i>
        </div>
        <div class="modal-body-custom">
          <span>Bạn có chắc chắn muốn {{ blockAction }} tài khoản này?</span>
        </div>
        <div class="modal-footer-custom">
          <button class="btn btn-light" @click="toggleBlockModal">Hủy</button>
          <button class="btn btn-danger" @click="confirmBlockEmployee">
            Xác nhận
          </button>
        </div>
      </div>
    </div>

    <!-- Modal xác nhận xóa tài khoản  -->
    <div v-if="showDeleteModal" class="overlay">
      <div class="modal-custom">
        <div class="modal-title">
          <h4>Cảnh báo</h4>
          <i class="fa-solid fa-xmark" @click="toggleDeleteModal"></i>
        </div>
        <div class="modal-body-custom">
          <span>Bạn có chắc chắn muốn xóa tài khoản này?</span>
        </div>
        <div class="modal-footer-custom">
          <button class="btn btn-light" @click="toggleDeleteModal">Hủy</button>
          <button class="btn btn-danger" @click="confirmDeleteEmployee">
            Xác nhận
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from "vue";

const searchQuery = ref("");
const showEmployeeForm = ref(false);
const showBlockModal = ref(false);
const showDeleteModal = ref(false);
const blockAction = ref("");
const selectedEmployee = ref(null);
const isEditing = ref(false);
const recordsPerPage = ref(10);
const currentPage = ref(1);
const employeeForm = reactive({
  name: "",
  dateOfBirth: "",
  email: "",
  address: "",
  status: "active",
});

const employees = ref([
  {
    id: 1,
    name: "Nguyễn Văn A",
    dateOfBirth: "1990-02-28",
    email: "nvana@gmail.com",
    address: "Ba Đình, Hà Nội",
    status: "active",
  },
  {
    id: 2,
    name: "Trần Thị B",
    dateOfBirth: "1985-07-15",
    email: "ttb@gmail.com",
    address: "Cầu Giấy, Hà Nội",
    status: "inactive",
  },
]);

const filteredEmployees = computed(() => {
  return employees.value.filter((emp) => emp.email.includes(searchQuery.value));
});

const totalPages = computed(() => {
  return Math.ceil(filteredEmployees.value.length / recordsPerPage.value);
});

const toggleEmployeeForm = () => {
  showEmployeeForm.value = !showEmployeeForm.value;
  isEditing.value = false;
  resetForm();
};

const resetSearch = () => {
  searchQuery.value = "";
};

const saveEmployee = () => {
  if (isEditing.value) {
    Object.assign(selectedEmployee.value, employeeForm);
  } else {
    employees.value.push({ ...employeeForm, id: Date.now() });
  }
  toggleEmployeeForm();
};

const editEmployee = (employee) => {
  selectedEmployee.value = employee;
  Object.assign(employeeForm, employee);
  isEditing.value = true;
  toggleEmployeeForm();
};

const deleteEmployee = (employee) => {
  selectedEmployee.value = employee;
  toggleDeleteModal();
};

const confirmDeleteEmployee = () => {
  employees.value = employees.value.filter(
    (emp) => emp.id !== selectedEmployee.value.id
  );
  toggleDeleteModal();
};

const toggleBlockEmployee = (employee) => {
  selectedEmployee.value = employee;
  blockAction.value = employee.status === "active" ? "chặn" : "bỏ chặn";
  toggleBlockModal();
};

const confirmBlockEmployee = () => {
  selectedEmployee.value.status =
    selectedEmployee.value.status === "active" ? "inactive" : "active";
  toggleBlockModal();
};

const toggleBlockModal = () => {
  showBlockModal.value = !showBlockModal.value;
};

const toggleDeleteModal = () => {
  showDeleteModal.value = !showDeleteModal.value;
};

const resetForm = () => {
  employeeForm.name = "";
  employeeForm.dateOfBirth = "";
  employeeForm.email = "";
  employeeForm.address = "";
};

const setPage = (page) => {
  currentPage.value = page;
};

const nextPage = () => {
  if (currentPage.value < totalPages.value) currentPage.value += 1;
};

const previousPage = () => {
  if (currentPage.value > 1) currentPage.value -= 1;
};
</script>

<style>
</style>
